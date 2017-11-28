---
title: "Notification Hubs biztonságos leküldéses aaaAzure"
description: "Tudnivalók a biztonságos toosend a leküldéses értesítések az Azure-ban. A Kódminták C# hello .NET API használatával."
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: b6fe16c96d28d75ff698278409fb012472ba6396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="2ca52-104">Biztonságos Azure Notification Hubs leküldéses</span><span class="sxs-lookup"><span data-stu-id="2ca52-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2ca52-105">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="2ca52-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="2ca52-106">iOS</span><span class="sxs-lookup"><span data-stu-id="2ca52-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="2ca52-107">Android</span><span class="sxs-lookup"><span data-stu-id="2ca52-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="2ca52-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2ca52-108">Overview</span></span>
<span data-ttu-id="2ca52-109">Leküldéses értesítési támogatása a Microsoft Azure lehetővé teszi tooaccess egy könnyen használható, többplatformos, kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok.</span><span class="sxs-lookup"><span data-stu-id="2ca52-109">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="2ca52-110">Tooregulatory vagy biztonsági korlátozások miatt néha egy alkalmazás érdemes tooinclude valamit a hello szabványos leküldéses értesítési infrastruktúrát keresztül nem továbbítható hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="2ca52-110">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="2ca52-111">Ez az oktatóanyag leírja, hogyan tooachieve hello úgy, hogy a bizalmas adatokat hello ügyféleszköz- és hello háttéralkalmazás közötti biztonságos és hitelesített kapcsolaton keresztül küld ugyanazt a felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="2ca52-111">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client device and hello app backend.</span></span>

<span data-ttu-id="2ca52-112">Magas szinten hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="2ca52-112">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="2ca52-113">hello app háttér:</span><span class="sxs-lookup"><span data-stu-id="2ca52-113">hello app back-end:</span></span>
   * <span data-ttu-id="2ca52-114">Háttér-adatbázisban tárolja biztonságos hasznos.</span><span class="sxs-lookup"><span data-stu-id="2ca52-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="2ca52-115">Küldi hello azonosítója (nem biztonságos információk küldése) értesítési toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="2ca52-115">Sends hello ID of this notification toohello device (no secure information is sent).</span></span>
2. <span data-ttu-id="2ca52-116">hello alkalmazást hello eszközön, hello értesítés fogadása közben:</span><span class="sxs-lookup"><span data-stu-id="2ca52-116">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="2ca52-117">hello eszköz kapcsolatba lép a hello háttér-kérelmező hello biztonságos hasznos.</span><span class="sxs-lookup"><span data-stu-id="2ca52-117">hello device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="2ca52-118">hello hasznos mutatja az hello app értesítésként hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="2ca52-118">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="2ca52-119">Fontos, hogy megelőző folyamata hello (és az oktatóanyag) feltételezzük, hogy hello eszköz toonote tárol egy hitelesítési jogkivonatot helyi tárhelyre – hello felhasználó bejelentkezése után.</span><span class="sxs-lookup"><span data-stu-id="2ca52-119">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="2ca52-120">Ez biztosítja, hogy teljesen zökkenőmentes élményt, mivel hello eszköz hello értesítési biztonságos hasznos a token használatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="2ca52-120">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="2ca52-121">Ha az alkalmazás nem tárolja a hitelesítési tokenek hello eszköz, vagy ezeket a jogkivonatokat is járhatott, hello eszközalkalmazás hello értesítés fogadásakor megjelenjen-e arra kéri a hello felhasználói toolaunch hello alkalmazás általános értesítést.</span><span class="sxs-lookup"><span data-stu-id="2ca52-121">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="2ca52-122">hello app majd hello felhasználó hitelesíti, és hello értesítési tartalom jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2ca52-122">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="2ca52-123">Ez biztonságos leküldéses az oktatóanyag bemutatja, hogyan toosend leküldéses értesítés biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="2ca52-123">This Secure Push tutorial shows how toosend a push notification securely.</span></span> <span data-ttu-id="2ca52-124">hello oktatóanyag épít, hello [felhasználók értesítése](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) oktatóanyag, ezért el kell végeznie hello lépéseket, hogy az oktatóanyagban először.</span><span class="sxs-lookup"><span data-stu-id="2ca52-124">hello tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tutorial, so you should complete hello steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="2ca52-125">Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (a Windows áruházban)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span><span class="sxs-lookup"><span data-stu-id="2ca52-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span></span>
> <span data-ttu-id="2ca52-126">Emellett vegye figyelembe, hogy a Windows Phone 8.1 (nem Windows Phone) Windows hitelesítő adatok szükségesek, és hogy háttérfeladatok nem működnek a Windows Phone 8.0-s vagy Silverlight 8.1.</span><span class="sxs-lookup"><span data-stu-id="2ca52-126">Also, note that Windows Phone 8.1 requires Windows (not Windows Phone) credentials, and that background tasks do not work on Windows Phone 8.0 or Silverlight 8.1.</span></span> <span data-ttu-id="2ca52-127">A Windows Áruházbeli alkalmazásokhoz is fogadhatja az értesítéseket keresztül háttérfeladat csak akkor, ha hello app zárolási képernyő engedélyezve (jelölőnégyzettel hello a hello Appmanifest).</span><span class="sxs-lookup"><span data-stu-id="2ca52-127">For Windows Store applications, you can receive notifications via a background task only if hello app is lock-screen enabled (click hello checkbox in hello Appmanifest).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-windows-phone-project"></a><span data-ttu-id="2ca52-128">Windows Phone-projekt hello módosítása</span><span class="sxs-lookup"><span data-stu-id="2ca52-128">Modify hello Windows Phone Project</span></span>
1. <span data-ttu-id="2ca52-129">A hello **NotifyUserWindowsPhone** projektre, adja hozzá a következő kód tooApp.xaml.cs tooregister hello leküldéses háttérfeladat hello.</span><span class="sxs-lookup"><span data-stu-id="2ca52-129">In hello **NotifyUserWindowsPhone** project, add hello following code tooApp.xaml.cs tooregister hello push background task.</span></span> <span data-ttu-id="2ca52-130">Adja hozzá a következő kódsort hello hello végén hello `OnLaunched()` módszert:</span><span class="sxs-lookup"><span data-stu-id="2ca52-130">Add hello following line of code at hello end of hello `OnLaunched()` method:</span></span>
   
        RegisterBackgroundTask();
2. <span data-ttu-id="2ca52-131">Továbbra is az App.xaml.cs fájlban adja hozzá a következő kódot közvetlenül után hello hello `OnLaunched()` módszert:</span><span class="sxs-lookup"><span data-stu-id="2ca52-131">Still in App.xaml.cs, add hello following code immediately after hello `OnLaunched()` method:</span></span>
   
        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();
   
                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }
3. <span data-ttu-id="2ca52-132">Adja hozzá a következő hello `using` hello App.xaml.cs fájlt hello tetején utasításokat:</span><span class="sxs-lookup"><span data-stu-id="2ca52-132">Add hello following `using` statements at hello top of hello App.xaml.cs file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. <span data-ttu-id="2ca52-133">A hello **fájl** a Visual Studio menüjében kattintson **összes mentése**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-133">From hello **File** menu in Visual Studio, click **Save All**.</span></span>

## <a name="create-hello-push-background-component"></a><span data-ttu-id="2ca52-134">Hello leküldéses háttér-összetevő létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ca52-134">Create hello Push Background Component</span></span>
<span data-ttu-id="2ca52-135">hello következő lépés egy toocreate hello leküldéses háttér összetevő.</span><span class="sxs-lookup"><span data-stu-id="2ca52-135">hello next step is toocreate hello push background component.</span></span>

1. <span data-ttu-id="2ca52-136">A Megoldáskezelőben kattintson jobb gombbal a legfelső szintű csomópontja hello hello megoldás (**megoldás SecurePush** ebben az esetben), majd kattintson a **Hozzáadás**, majd kattintson **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-136">In Solution Explorer, right-click hello top-level node of hello solution (**Solution SecurePush** in this case), then click **Add**, then click **New Project**.</span></span>
2. <span data-ttu-id="2ca52-137">Bontsa ki a **Áruházbeli alkalmazások**, majd kattintson a **Windows Phone-alkalmazások**, kattintson a **Windows-Futtatókörnyezetű összetevőben (Windows Phone)**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-137">Expand **Store Apps**, then click **Windows Phone Apps**, then click **Windows Runtime Component (Windows Phone)**.</span></span> <span data-ttu-id="2ca52-138">Név hello projekt **PushBackgroundComponent**, és kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="2ca52-138">Name hello project **PushBackgroundComponent**, and then click **OK** toocreate hello project.</span></span>
   
    ![][12]
3. <span data-ttu-id="2ca52-139">A Megoldáskezelőben kattintson a jobb gombbal hello **PushBackgroundComponent (Windows Phone 8.1)** projektre, majd kattintson a **Hozzáadás**, majd kattintson a **osztály**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-139">In Solution Explorer, right-click hello **PushBackgroundComponent (Windows Phone 8.1)** project, then click **Add**, then click **Class**.</span></span> <span data-ttu-id="2ca52-140">Hello új osztály neve **PushBackgroundTask.cs**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-140">Name hello new class **PushBackgroundTask.cs**.</span></span> <span data-ttu-id="2ca52-141">Kattintson a **Hozzáadás** toogenerate hello osztály.</span><span class="sxs-lookup"><span data-stu-id="2ca52-141">Click **Add** toogenerate hello class.</span></span>
4. <span data-ttu-id="2ca52-142">Cserélje le a teljes tartalma hello hello **PushBackgroundComponent** névtér-definíciót az alábbi kódot, és hello helyőrző hello `{back-end endpoint}` hello háttér-végponttal másikban kapott a Háttér:</span><span class="sxs-lookup"><span data-stu-id="2ca52-142">Replace hello entire contents of hello **PushBackgroundComponent** namespace definition with hello following code, substituting hello placeholder `{back-end endpoint}` with hello back-end endpoint obtained while deploying your back-end:</span></span>
   
        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }
   
            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";
   
                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store hello content received from hello notification so it can be retrieved from hello UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;
   
                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);
   
                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);
   
                    ShowToast(notification);
   
                    deferral.Complete();
                }
   
                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }
5. <span data-ttu-id="2ca52-143">A Megoldáskezelőben kattintson a jobb gombbal hello **PushBackgroundComponent (Windows Phone 8.1)** projektre, majd kattintson **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-143">In Solution Explorer, right-click hello **PushBackgroundComponent (Windows Phone 8.1)** project and then click **Manage NuGet Packages**.</span></span>
6. <span data-ttu-id="2ca52-144">Kattintson a bal oldalon hello **Online**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-144">On hello left-hand side, click **Online**.</span></span>
7. <span data-ttu-id="2ca52-145">A hello **keresési** mezőbe írja be **HTTP-alapú**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-145">In hello **Search** box, type **Http Client**.</span></span>
8. <span data-ttu-id="2ca52-146">Hello eredmények listájában kattintson **Microsoft HTTP ügyfél függvénytárainak**, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-146">In hello results list, click **Microsoft HTTP Client Libraries**, and then click **Install**.</span></span> <span data-ttu-id="2ca52-147">Hello telepítését.</span><span class="sxs-lookup"><span data-stu-id="2ca52-147">Complete hello installation.</span></span>
9. <span data-ttu-id="2ca52-148">Vissza a hello NuGet **keresési** mezőbe írja be **Json.net**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-148">Back in hello NuGet **Search** box, type **Json.net**.</span></span> <span data-ttu-id="2ca52-149">Telepítse a hello **Json.NET** csomagot, majd Bezárás hello NuGet Package Manager ablak.</span><span class="sxs-lookup"><span data-stu-id="2ca52-149">Install hello **Json.NET** package, then close hello NuGet Package Manager window.</span></span>
10. <span data-ttu-id="2ca52-150">Adja hozzá a következő hello `using` hello hello tetején utasítások **PushBackgroundTask.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="2ca52-150">Add hello following `using` statements at hello top of hello **PushBackgroundTask.cs** file:</span></span>
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. <span data-ttu-id="2ca52-151">A Megoldáskezelőben a hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projektre, kattintson a jobb gombbal **hivatkozások**, majd kattintson a **hivatkozás hozzáadása...** . Hello hivatkozáskezelő párbeszédpanelen jelölőnégyzetet hello mellett túl**PushBackgroundComponent**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-151">In Solution Explorer, in hello **NotifyUserWindowsPhone (Windows Phone 8.1)** project, right-click **References**, then click **Add Reference...**. In hello Reference Manager dialog, check hello box next too**PushBackgroundComponent**, and then click **OK**.</span></span>
12. <span data-ttu-id="2ca52-152">A Megoldáskezelőben kattintson duplán a **Package.appxmanifest** a hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt.</span><span class="sxs-lookup"><span data-stu-id="2ca52-152">In Solution Explorer, double-click **Package.appxmanifest** in hello **NotifyUserWindowsPhone (Windows Phone 8.1)** project.</span></span> <span data-ttu-id="2ca52-153">A **értesítések**, beállíthatja **bejelentési képes** túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-153">Under **Notifications**, set **Toast Capable** too**Yes**.</span></span>
    
    ![][3]
13. <span data-ttu-id="2ca52-154">Még mindig **Package.appxmanifest**, kattintson a hello **nyilatkozatok** hello felső menüből.</span><span class="sxs-lookup"><span data-stu-id="2ca52-154">Still in **Package.appxmanifest**, click hello **Declarations** menu near hello top.</span></span> <span data-ttu-id="2ca52-155">A hello **elérhető nyilatkozatok** legördülő menüben kattintson a **háttérfeladatok**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-155">In hello **Available Declarations** dropdown, click **Background Tasks**, and then click **Add**.</span></span>
14. <span data-ttu-id="2ca52-156">A **Package.appxmanifest**a **tulajdonságok**, ellenőrizze **leküldéses értesítési**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-156">In **Package.appxmanifest**, under **Properties**, check **Push notification**.</span></span>
15. <span data-ttu-id="2ca52-157">A **Package.appxmanifest**a **Alkalmazásbeállítások**, típus **PushBackgroundComponent.PushBackgroundTask** a hello **belépési pont** a mező.</span><span class="sxs-lookup"><span data-stu-id="2ca52-157">In **Package.appxmanifest**, under **App Settings**, type **PushBackgroundComponent.PushBackgroundTask** in hello **Entry Point** field.</span></span>
    
    ![][13]
16. <span data-ttu-id="2ca52-158">A hello **fájl** menüben kattintson a **összes mentése**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-158">From hello **File** menu, click **Save All**.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="2ca52-159">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="2ca52-159">Run hello Application</span></span>
<span data-ttu-id="2ca52-160">toorun hello alkalmazás, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2ca52-160">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="2ca52-161">A Visual Studio, futtassa a hello **AppBackend** webes API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2ca52-161">In Visual Studio, run hello **AppBackend** Web API application.</span></span> <span data-ttu-id="2ca52-162">Az ASP.NET-weblap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2ca52-162">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="2ca52-163">A Visual Studio, futtassa a hello **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2ca52-163">In Visual Studio, run hello **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone app.</span></span> <span data-ttu-id="2ca52-164">Windows Phone-emulátor hello fut, és automatikusan betölti hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2ca52-164">hello Windows Phone emulator runs and loads hello app automatically.</span></span>
3. <span data-ttu-id="2ca52-165">A hello **NotifyUserWindowsPhone** alkalmazás felhasználói felületén, adja meg a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="2ca52-165">In hello **NotifyUserWindowsPhone** app UI, enter a username and password.</span></span> <span data-ttu-id="2ca52-166">Ezek karakterlánc lehet, de kell hello ugyanazt az értéket.</span><span class="sxs-lookup"><span data-stu-id="2ca52-166">These can be any string, but they must be hello same value.</span></span>
4. <span data-ttu-id="2ca52-167">A hello **NotifyUserWindowsPhone** alkalmazás felhasználói felületén kattintson **jelentkezzen be, és regisztrálja**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-167">In hello **NotifyUserWindowsPhone** app UI, click **Log in and register**.</span></span> <span data-ttu-id="2ca52-168">Kattintson a **leküldéses küldése**.</span><span class="sxs-lookup"><span data-stu-id="2ca52-168">Then click **Send push**.</span></span>

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
