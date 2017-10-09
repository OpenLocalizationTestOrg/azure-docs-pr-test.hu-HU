---
title: "aaaAzure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel"
description: "Tudnivalók a biztonságos toosend a leküldéses értesítések az Azure-ban. A Kódminták C# hello .NET API használatával."
documentationcenter: windows
author: ysxu
manager: erikre
services: notification-hubs
editor: 
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: a366181faa81e78adf4de61435ef2790c3aa29d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a><span data-ttu-id="fda0f-104">Az Azure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel</span><span class="sxs-lookup"><span data-stu-id="fda0f-104">Azure Notification Hubs Notify Users with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="fda0f-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fda0f-105">Overview</span></span>
<span data-ttu-id="fda0f-106">Leküldéses értesítési támogatás az Azure-ban lehetővé teszi tooaccess egy könnyen kezelhető, multiplatform és kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok.</span><span class="sxs-lookup"><span data-stu-id="fda0f-106">Push notification support in Azure enables you tooaccess an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="fda0f-107">Ez az oktatóanyag toouse Azure Notification Hubs toosend a leküldéses értesítések tooa adott alkalmazás felhasználói az adott eszközön.</span><span class="sxs-lookup"><span data-stu-id="fda0f-107">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa specific app user on a specific device.</span></span> <span data-ttu-id="fda0f-108">ASP.NET WebAPI háttérrendszerből használt tooauthenticate ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="fda0f-108">An ASP.NET WebAPI backend is used tooauthenticate clients.</span></span> <span data-ttu-id="fda0f-109">Ügyfél felhasználói hello segítségével hitelesíti, és címke hello háttér toonotification regisztrációs automatikusan felveszi.</span><span class="sxs-lookup"><span data-stu-id="fda0f-109">Using hello authenticated client user, and tag will be automatically added by hello backend toonotification registration.</span></span> <span data-ttu-id="fda0f-110">Ezt a címkét hello háttér toogenerate értesítések egy adott felhasználó által használt toosend lesz.</span><span class="sxs-lookup"><span data-stu-id="fda0f-110">This tag will be used toosend by hello backend toogenerate notifications for a specific user.</span></span> <span data-ttu-id="fda0f-111">További információ a Apps-háttéralkalmazás segítségével értesítések regisztrálása a témakörben hello útmutatást [az alkalmazás háttérrendszeréből regisztrálása](http://msdn.microsoft.com/library/dn743807.aspx).</span><span class="sxs-lookup"><span data-stu-id="fda0f-111">For more information on registering for notifications using an app backend, see hello guidance topic [Registering from your app backend](http://msdn.microsoft.com/library/dn743807.aspx).</span></span> <span data-ttu-id="fda0f-112">Ez az oktatóanyag épít hello értesítési központ és az hello létrehozott projekt [Ismerkedés a Notification Hubs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fda0f-112">This tutorial builds on hello notification hub and project that you created in hello [Get started with Notification Hubs] tutorial.</span></span>

<span data-ttu-id="fda0f-113">Ez az oktatóanyag egyben hello előfeltétel toohello [biztonságos leküldéses] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fda0f-113">This tutorial is also hello prerequisite toohello [Secure Push] tutorial.</span></span> <span data-ttu-id="fda0f-114">Hello lépéseit az oktatóanyag befejezése után folytathatja toohello [biztonságos leküldéses] oktatóanyag, amely bemutatja, hogyan toomodify hello kódot az oktatóanyag toosend leküldéses értesítés biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="fda0f-114">After you have completed hello steps in this tutorial, you can proceed toohello [Secure Push] tutorial, which shows how toomodify hello code in this tutorial toosend a push notification securely.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fda0f-115">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="fda0f-115">Before you begin</span></span>
<span data-ttu-id="fda0f-116">Visszajelzéseit komolyan vesszük.</span><span class="sxs-lookup"><span data-stu-id="fda0f-116">We do take your feedback seriously.</span></span> <span data-ttu-id="fda0f-117">Ha bármilyen nehézségbe ütközik a témakör, vagy javaslatai vannak, a tartalom javítása befejezése, azt fogadjuk visszajelzéseit hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="fda0f-117">If you have any difficulties completing this topic, or recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span>

<span data-ttu-id="fda0f-118">az oktatóanyag befejezése hello kódja a Githubon található [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span><span class="sxs-lookup"><span data-stu-id="fda0f-118">hello completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fda0f-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fda0f-119">Prerequisites</span></span>
<span data-ttu-id="fda0f-120">Ez az oktatóanyag elindítása előtt már végrehajtotta a Mobile Services-oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="fda0f-120">Before you start this tutorial, you must have already completed these Mobile Services tutorials:</span></span>

* <span data-ttu-id="fda0f-121">[Ismerkedés a Notification Hubs]</span><span class="sxs-lookup"><span data-stu-id="fda0f-121">[Get started with Notification Hubs]</span></span><br/><span data-ttu-id="fda0f-122">Az értesítési központ létrehozása és hello alkalmazásnév lefoglalása, és ebben az oktatóanyagban tooreceive értesítések regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="fda0f-122">You create your notification hub and reserve hello app name and register tooreceive notifications in this tutorial.</span></span> <span data-ttu-id="fda0f-123">Ez az oktatóanyag feltételezi, hogy már végrehajtotta ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="fda0f-123">This tutorial assumes you have already completed these steps.</span></span> <span data-ttu-id="fda0f-124">Ha nem, kövesse a hello lépéseket [Ismerkedés a Notification Hubs (a Windows áruházban)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); pontosabban szakaszok hello [hello Windows Áruházbeli alkalmazás regisztrálása](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) és [konfigurálása az értesítési központ](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span><span class="sxs-lookup"><span data-stu-id="fda0f-124">If not, please follow hello steps in [Getting Started with Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); specifically, hello sections [Register your app for hello Windows Store](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) and [Configure your Notification Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span></span> <span data-ttu-id="fda0f-125">Különösen, győződjön meg arról, hogy megadta-e hello **CSOMAGAZONOSÍTÓT** és **Ügyfélkulcs** hello portálon, a hello értékek **konfigurálása** az értesítési központ lapján.</span><span class="sxs-lookup"><span data-stu-id="fda0f-125">In particular, make sure that you have entered hello **Package SID** and **Client Secret** values in hello portal, in hello **Configure** tab for your notification hub.</span></span> <span data-ttu-id="fda0f-126">A konfigurációs eljárást hello szakasz [az értesítési központ konfigurálása](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span><span class="sxs-lookup"><span data-stu-id="fda0f-126">This configuration procedure is described in hello section [Configure your Notification Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span></span> <span data-ttu-id="fda0f-127">Ez az egy fontos lépés: Ha hello portal hello-felhasználó hitelesítő adatai nem egyezik meg a hello alkalmazás neve mellett dönt, hello leküldéses értesítés nem fog sikerülni.</span><span class="sxs-lookup"><span data-stu-id="fda0f-127">This is an important step: if hello credentials on hello portal do not match those specified for hello app name you choose, hello push notification will not succeed.</span></span>

> [!NOTE]
> <span data-ttu-id="fda0f-128">Ha a Mobile Apps az Azure App Service-ben, a háttérszolgáltatás használ, tekintse meg a hello [Mobile Apps-verziójáért](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fda0f-128">If you are using Mobile Apps in Azure App Service as your backend service, see hello [Mobile Apps version](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) of this tutorial.</span></span>
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-hello-code-for-hello-client-project"></a><span data-ttu-id="fda0f-129">Hello ügyfélprojekt hello kódját frissítése</span><span class="sxs-lookup"><span data-stu-id="fda0f-129">Update hello code for hello client project</span></span>
<span data-ttu-id="fda0f-130">Ebben a szakaszban hello elvégezte hello projektben hello kód frissítése [Ismerkedés a Notification Hubs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fda0f-130">In this section, you update hello code in hello project you completed for hello [Get started with Notification Hubs] tutorial.</span></span> <span data-ttu-id="fda0f-131">hello már kell hello áruházhoz kapcsolódó és az értesítési központ konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="fda0f-131">hello should already be associated with hello store and configured for your notification hub.</span></span> <span data-ttu-id="fda0f-132">Ebben a szakaszban kód toocall hello új WebAPI háttérrendszerből hozzáadása, és a regisztráció és értesítések küldésének használni.</span><span class="sxs-lookup"><span data-stu-id="fda0f-132">In this section, you will add code toocall hello new WebAPI backend and use it for registering and sending notifications.</span></span>

1. <span data-ttu-id="fda0f-133">A Visual Studióban nyissa meg a hello hello megoldást hello létrehozott [Ismerkedés a Notification Hubs] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fda0f-133">In Visual Studio, open hello hello solution you created for hello [Get started with Notification Hubs] tutorial.</span></span>
2. <span data-ttu-id="fda0f-134">A Megoldáskezelőben kattintson a jobb gombbal hello **(Windows 8.1)** projektre, majd kattintson **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="fda0f-134">In Solution Explorer, right-click hello **(Windows 8.1)** project and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="fda0f-135">Kattintson a bal oldalon hello **Online**.</span><span class="sxs-lookup"><span data-stu-id="fda0f-135">On hello left-hand side, click **Online**.</span></span>
4. <span data-ttu-id="fda0f-136">A hello **keresési** mezőbe írja be **HTTP-alapú**.</span><span class="sxs-lookup"><span data-stu-id="fda0f-136">In hello **Search** box, type **Http Client**.</span></span>
5. <span data-ttu-id="fda0f-137">Hello eredmények listájában kattintson **Microsoft HTTP ügyfél függvénytárainak**, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="fda0f-137">In hello results list, click **Microsoft HTTP Client Libraries**, and then click **Install**.</span></span> <span data-ttu-id="fda0f-138">Hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="fda0f-138">Complete hello installation.</span></span>
6. <span data-ttu-id="fda0f-139">Vissza a hello NuGet **keresési** mezőbe írja be **Json.net**.</span><span class="sxs-lookup"><span data-stu-id="fda0f-139">Back in hello NuGet **Search** box, type **Json.net**.</span></span> <span data-ttu-id="fda0f-140">Telepítse a hello **Json.NET** csomagot, és majd Bezárás hello NuGet Package Manager ablak.</span><span class="sxs-lookup"><span data-stu-id="fda0f-140">Install hello **Json.NET** package, and then close hello NuGet Package Manager window.</span></span>
7. <span data-ttu-id="fda0f-141">Ismételje meg hello fent hello **(Windows Phone 8.1)** projekt tooinstall hello **JSON.NET** hello Windows Phone-projektet a NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="fda0f-141">Repeat hello steps above for hello **(Windows Phone 8.1)** project tooinstall hello **JSON.NET** NuGet package for hello Windows Phone project.</span></span>
8. <span data-ttu-id="fda0f-142">A Megoldáskezelőben a hello **(Windows 8.1)** projektre, kattintson duplán a **MainPage.xaml** tooopen azt hello Visual Studio szerkesztőjében.</span><span class="sxs-lookup"><span data-stu-id="fda0f-142">In Solution Explorer, in hello **(Windows 8.1)** project, double-click **MainPage.xaml** tooopen it in hello Visual Studio editor.</span></span>
9. <span data-ttu-id="fda0f-143">A hello **MainPage.xaml** XML-kódot, a név felülírandó hello `<Grid>` hello kód a következő szakasz ismerteti.</span><span class="sxs-lookup"><span data-stu-id="fda0f-143">In hello **MainPage.xaml** XML code, replace hello `<Grid>` section with hello following code.</span></span> <span data-ttu-id="fda0f-144">Ez a kód hozzáadása a felhasználónév és jelszó szövegmezőben, amely hello felhasználó hitelesíti magát az.</span><span class="sxs-lookup"><span data-stu-id="fda0f-144">This code adds a username and password textbox that hello user will authenticate with.</span></span> <span data-ttu-id="fda0f-145">Bővíti ezenkívül a szövegmezőből hello értesítési üzenet és hello felhasználónév címke, amely hello értesítést kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="fda0f-145">It also adds textboxes for hello notification message and hello username tag that should receive hello notification:</span></span>
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
   
            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>
   
            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
   
                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>
   
                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />
   
                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag tooSend To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>
10. <span data-ttu-id="fda0f-146">A Megoldáskezelőben a hello **(Windows Phone 8.1)** projektben nyissa meg **MainPage.xaml** , és cserélje le a Windows Phone 8.1 hello `<Grid>` szakasz ismerteti, hogy ugyanazt a fenti kódot.</span><span class="sxs-lookup"><span data-stu-id="fda0f-146">In Solution Explorer, in hello **(Windows Phone 8.1)** project, open **MainPage.xaml** and replace hello Windows Phone 8.1 `<Grid>` section with that same code above.</span></span> <span data-ttu-id="fda0f-147">hello felület alábbihoz hasonló toowhats alább látható.</span><span class="sxs-lookup"><span data-stu-id="fda0f-147">hello interface should look similar toowhats shown below.</span></span>
    
    ![][13]
11. <span data-ttu-id="fda0f-148">A Solution Explorerben nyissa meg a hello **MainPage.xaml.cs** hello fájlt **(Windows 8.1)** és **(Windows Phone 8.1)** projektek.</span><span class="sxs-lookup"><span data-stu-id="fda0f-148">In Solution Explorer, open hello **MainPage.xaml.cs** file for hello **(Windows 8.1)** and **(Windows Phone 8.1)** projects.</span></span> <span data-ttu-id="fda0f-149">Adja hozzá a következő hello `using` hello felső fájlokra utasításokat:</span><span class="sxs-lookup"><span data-stu-id="fda0f-149">Add hello following `using` statements at hello top of both files:</span></span>
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. <span data-ttu-id="fda0f-150">A **MainPage.xaml.cs** a hello **(Windows 8.1)** és **(Windows Phone 8.1)** projektek, adja hozzá a következő tag toohello hello `MainPage` osztály.</span><span class="sxs-lookup"><span data-stu-id="fda0f-150">In **MainPage.xaml.cs** for hello **(Windows 8.1)** and **(Windows Phone 8.1)** projects, add hello following member toohello `MainPage` class.</span></span> <span data-ttu-id="fda0f-151">Lehet, hogy tooreplace `<Enter Your Backend Endpoint>` a hello a tényleges háttér-végpontot szerezte be korábban.</span><span class="sxs-lookup"><span data-stu-id="fda0f-151">Be sure tooreplace `<Enter Your Backend Endpoint>` with hello your actual backend endpoint obtained previously.</span></span> <span data-ttu-id="fda0f-152">Például: `http://mybackend.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="fda0f-152">For example, `http://mybackend.azurewebsites.net`.</span></span>
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. <span data-ttu-id="fda0f-153">Adja hozzá hello kódot alább toohello MainPage osztály **MainPage.xaml.cs** a hello **(Windows 8.1)** és **(Windows Phone 8.1)** projektek.</span><span class="sxs-lookup"><span data-stu-id="fda0f-153">Add hello code below toohello MainPage class in **MainPage.xaml.cs** for hello **(Windows 8.1)** and **(Windows Phone 8.1)** projects.</span></span>
    
    <span data-ttu-id="fda0f-154">Hello `PushClick` metódus hello hello kezelője kattintson **küldése leküldéses** gombra.</span><span class="sxs-lookup"><span data-stu-id="fda0f-154">hello `PushClick` method is hello click handler for hello **Send Push** button.</span></span> <span data-ttu-id="fda0f-155">Eszközök címkével ellátott felhasználónév, amely megfelel a hello meghívja hello háttér tootrigger egy értesítési tooall `to_tag` paraméter.</span><span class="sxs-lookup"><span data-stu-id="fda0f-155">It calls hello backend tootrigger a notification tooall devices with a username tag that matches hello `to_tag` parameter.</span></span> <span data-ttu-id="fda0f-156">hello értesítési üzenettel JSON-tartalomként hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="fda0f-156">hello notification message is sent as JSON content in hello request body.</span></span>
    
    <span data-ttu-id="fda0f-157">Hello `LoginAndRegisterClick` metódus hello hello kezelője kattintson **jelentkezzen be, és regisztrálja** gombra.</span><span class="sxs-lookup"><span data-stu-id="fda0f-157">hello `LoginAndRegisterClick` method is hello click handler for hello **Log in and register** button.</span></span> <span data-ttu-id="fda0f-158">Alapszintű hello tárol hitelesítési jogkivonat a helyi tároló (vegye figyelembe, hogy a hitelesítési sémát használja bármely jogkivonat jelképez), majd használja `RegisterClient` tooregister hello backend használata az értesítésekhez.</span><span class="sxs-lookup"><span data-stu-id="fda0f-158">It stores hello basic authentication token in local storage (note that this represents any token your authentication scheme uses), then uses `RegisterClient` tooregister for notifications using hello backend.</span></span>

        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed toosend " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // hello "username:<user name>" tag gets automatically added by hello message handler in hello backend.
            // hello tag passed here can be whatever other tags you may want toouse.
            try
            {
                // hello device handle used will be different depending on hello device and PNS. 
                // Windows devices use hello channel uri as hello PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed tooregister with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



1. <span data-ttu-id="fda0f-159">A Megoldáskezelőben a hello **megosztott** projektet, nyissa meg hello **App.xaml.cs** fájlt.</span><span class="sxs-lookup"><span data-stu-id="fda0f-159">In Solution Explorer, under hello **Shared** project, open hello **App.xaml.cs** file.</span></span> <span data-ttu-id="fda0f-160">Keresése túl hello hívás`InitNotificationsAsync()` a hello `OnLaunched()` eseménykezelő.</span><span class="sxs-lookup"><span data-stu-id="fda0f-160">Find hello call too`InitNotificationsAsync()` in hello `OnLaunched()` event handler.</span></span> <span data-ttu-id="fda0f-161">Megjegyzéssé vagy hello hívás túl törlése`InitNotificationsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="fda0f-161">Comment out or delete hello call too`InitNotificationsAsync()`.</span></span> <span data-ttu-id="fda0f-162">hello gomb kezelő fent inicializálja értesítési regisztráció.</span><span class="sxs-lookup"><span data-stu-id="fda0f-162">hello button handler added above will initialize notification registrations.</span></span>

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. <span data-ttu-id="fda0f-163">A Megoldáskezelőben kattintson a jobb gombbal hello **megosztott** projektre, majd kattintson a **Hozzáadás**, és kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="fda0f-163">In Solution Explorer, right-click hello **Shared** project, then click **Add**, and then click **Class**.</span></span> <span data-ttu-id="fda0f-164">Hello osztály neve **RegisterClient.cs**, majd kattintson a **OK** toogenerate hello osztály.</span><span class="sxs-lookup"><span data-stu-id="fda0f-164">Name hello class **RegisterClient.cs**, then click **OK** toogenerate hello class.</span></span>
   
   <span data-ttu-id="fda0f-165">Ez az osztály fog tegye hello REST hívások szükséges toocontact hello háttéralkalmazás, sorrendben tooregister leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="fda0f-165">This class will wrap hello REST calls required toocontact hello app backend, in order tooregister for push notifications.</span></span> <span data-ttu-id="fda0f-166">Helyileg tárolja a hello *registrationIds* hello által létrehozott értesítési központot, ahogy az az [az alkalmazás háttérrendszeréből regisztrálása](http://msdn.microsoft.com/library/dn743807.aspx).</span><span class="sxs-lookup"><span data-stu-id="fda0f-166">It also locally stores hello *registrationIds* created by hello Notification Hub as detailed in [Registering from your app backend](http://msdn.microsoft.com/library/dn743807.aspx).</span></span> <span data-ttu-id="fda0f-167">Vegye figyelembe, hogy használja-e egy engedélyezési jogkivonatot hello kattintva helyi storage-ban tárolt **jelentkezzen be, és regisztrálja** gombra.</span><span class="sxs-lookup"><span data-stu-id="fda0f-167">Note that it uses an authorization token stored in local storage when you click hello **Log in and register** button.</span></span>
2. <span data-ttu-id="fda0f-168">Adja hozzá a következő hello `using` hello RegisterClient.cs fájl hello tetején utasításokat:</span><span class="sxs-lookup"><span data-stu-id="fda0f-168">Add hello following `using` statements at hello top of hello RegisterClient.cs file:</span></span>
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. <span data-ttu-id="fda0f-169">Adja hozzá a következő kódot hello hello `RegisterClient` definition osztályban.</span><span class="sxs-lookup"><span data-stu-id="fda0f-169">Add hello following code inside hello `RegisterClient` class definition.</span></span>
   
       private string POST_URL;
   
       private class DeviceRegistration
       {
           public string Platform { get; set; }
           public string Handle { get; set; }
           public string[] Tags { get; set; }
       }
   
       public RegisterClient(string backendEndpoint)
       {
           POST_URL = backendEndpoint + "/api/register";
       }
   
       public async Task RegisterAsync(string handle, IEnumerable<string> tags)
       {
           var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
   
           var deviceRegistration = new DeviceRegistration
           {
               Platform = "wns",
               Handle = handle,
               Tags = tags.ToArray<string>()
           };
   
           var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
   
           if (statusCode == HttpStatusCode.Gone)
           {
               // regId is expired, deleting from local storage & recreating
               var settings = ApplicationData.Current.LocalSettings.Values;
               settings.Remove("__NHRegistrationId");
               regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
               statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
           }
   
           if (statusCode != HttpStatusCode.Accepted)
           {
               // log or throw
               throw new System.Net.WebException(statusCode.ToString());
           }
       }
   
       private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
       {
           using (var httpClient = new HttpClient())
           {
               var settings = ApplicationData.Current.LocalSettings.Values;
               httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);
   
               var putUri = POST_URL + "/" + regId;
   
               string json = JsonConvert.SerializeObject(deviceRegistration);
                               var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
               return response.StatusCode;
           }
       }
   
       private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
       {
           var settings = ApplicationData.Current.LocalSettings.Values;
           if (!settings.ContainsKey("__NHRegistrationId"))
           {
               using (var httpClient = new HttpClient())
               {
                   httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                   var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                   if (response.IsSuccessStatusCode)
                   {
                       string regId = await response.Content.ReadAsStringAsync();
                       regId = regId.Substring(1, regId.Length - 2);
                       settings.Add("__NHRegistrationId", regId);
                   }
                   else
                   {
                       throw new System.Net.WebException(response.StatusCode.ToString());
                   }
               }
           }
           return (string)settings["__NHRegistrationId"];
   
       }
4. <span data-ttu-id="fda0f-170">Mentse az összes módosítást.</span><span class="sxs-lookup"><span data-stu-id="fda0f-170">Save all your changes.</span></span>

## <a name="testing-hello-application"></a><span data-ttu-id="fda0f-171">Hello alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="fda0f-171">Testing hello Application</span></span>
1. <span data-ttu-id="fda0f-172">Indítsa el a Windows 8.1 és Windows Phone 8.1 hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fda0f-172">Launch hello application on both Windows 8.1 and Windows Phone 8.1.</span></span> <span data-ttu-id="fda0f-173">A Windows Phone 8.1-es futtathat hello példány hello emulátor vagy tényleges eszközön.</span><span class="sxs-lookup"><span data-stu-id="fda0f-173">For Windows Phone 8.1 you can run hello instance in hello emulator or an actual device.</span></span>
2. <span data-ttu-id="fda0f-174">A Windows 8.1 hello példány hello alkalmazás, írja be a **felhasználónév** és **jelszó** üdvözlő képernyőt az alábbi ábrán.</span><span class="sxs-lookup"><span data-stu-id="fda0f-174">In hello Windows 8.1 instance of hello app, enter a **Username** and **Password** as shown in hello screen below.</span></span> <span data-ttu-id="fda0f-175">Ez eltér a hello felhasználónevet és jelszót ad meg a Windows Phone kell.</span><span class="sxs-lookup"><span data-stu-id="fda0f-175">It should differ from hello user name and password you enter on Windows Phone.</span></span>
3. <span data-ttu-id="fda0f-176">Kattintson a **jelentkezzen be, és regisztrálja** , és ellenőrizze, hogy a párbeszédpanelen látható, hogy már bejelentkezett.</span><span class="sxs-lookup"><span data-stu-id="fda0f-176">Click **Log in and register** and verify a dialog shows that you have logged in.</span></span> <span data-ttu-id="fda0f-177">Ez azt is lehetővé teszi hello **küldése leküldéses** gombra.</span><span class="sxs-lookup"><span data-stu-id="fda0f-177">This will also enable hello **Send Push** button.</span></span>
   
    ![][14]
4. <span data-ttu-id="fda0f-178">Hello Windows Phone 8.1-példányon, adja meg a felhasználói név karakterláncát mindkét hello **felhasználónév** és **jelszó** mezők kattintson **bejelentkezési és regisztrációs**.</span><span class="sxs-lookup"><span data-stu-id="fda0f-178">On hello Windows Phone 8.1 instance, enter a user name string in both hello **Username** and **Password** fields then click **Login and register**.</span></span>
5. <span data-ttu-id="fda0f-179">Ezt a hello **címzett felhasználónév címke** mezőbe írja be a regisztrált Windows 8.1 hello felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="fda0f-179">Then in hello **Recipient Username Tag** field, enter hello user name registered on Windows 8.1.</span></span> <span data-ttu-id="fda0f-180">Adjon meg egy értesítési üzenetet, és kattintson a **küldése leküldéses**.</span><span class="sxs-lookup"><span data-stu-id="fda0f-180">Enter a notification message and click **Send Push**.</span></span>
   
    ![][16]
6. <span data-ttu-id="fda0f-181">Csak a regisztrált hello megfelelő felhasználónév címke hello eszközök hello értesítési üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="fda0f-181">Only hello devices that have registered with hello matching username tag receive hello notification message.</span></span>
   
    ![][15]

## <a name="next-steps"></a><span data-ttu-id="fda0f-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fda0f-182">Next Steps</span></span>
* <span data-ttu-id="fda0f-183">Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, lásd: [legfrissebb hírek Notification Hubs használata toosend].</span><span class="sxs-lookup"><span data-stu-id="fda0f-183">If you want toosegment your users by interest groups, see [Use Notification Hubs toosend breaking news].</span></span>
* <span data-ttu-id="fda0f-184">További kapcsolatos toolearn toouse Notification hubs használatával, lásd: [Notification Hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="fda0f-184">toolearn more about how toouse Notification Hubs, see [Notification Hubs Guidance].</span></span>

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Ismerkedés a Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[biztonságos leküldéses]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[legfrissebb hírek Notification Hubs használata toosend]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx
