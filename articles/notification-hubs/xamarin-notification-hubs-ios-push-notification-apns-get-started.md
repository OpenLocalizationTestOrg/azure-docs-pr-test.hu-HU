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
# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a><span data-ttu-id="fdaa3-104">iOS leküldéses értesítések küldése a Notification Hubs használatával Xamarin-alkalmazásokba</span><span class="sxs-lookup"><span data-stu-id="fdaa3-104">iOS Push Notifications with Notification Hubs for Xamarin apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="fdaa3-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fdaa3-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="fdaa3-106">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="fdaa3-107">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="fdaa3-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="fdaa3-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="fdaa3-109">Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooan iOS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooan iOS application.</span></span>
<span data-ttu-id="fdaa3-110">Lesz hozzon létre egy üres Xamarin.iOS-alkalmazást, amely leküldéses értesítéseket fogad hello segítségével [Apple Push Notification szolgáltatás (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).</span><span class="sxs-lookup"><span data-stu-id="fdaa3-110">You'll create a blank Xamarin.iOS app that receives push notifications by using hello [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).</span></span> <span data-ttu-id="fdaa3-111">Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-111">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span> <span data-ttu-id="fdaa3-112">hello befejezett kód is elérhető hello [NotificationHubs-alkalmazásban] [ GitHub] minta.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-112">hello finished code is available in hello [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="fdaa3-113">Ez az oktatóanyag bemutatja, hogyan hello egyszerű leküldéses üzenetküldési forgatókönyvet a Notification hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-113">This tutorial demonstrates hello simple push message broadcast scenario with Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fdaa3-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fdaa3-114">Prerequisites</span></span>
<span data-ttu-id="fdaa3-115">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-115">This tutorial requires hello following:</span></span>

* <span data-ttu-id="fdaa3-116">[XCode 6.0][Install Xcode]</span><span class="sxs-lookup"><span data-stu-id="fdaa3-116">[Xcode 6.0][Install Xcode]</span></span>
* <span data-ttu-id="fdaa3-117">Az iOS 7.0-tel (vagy újabb verzióval) kompatibilis eszköz</span><span class="sxs-lookup"><span data-stu-id="fdaa3-117">An iOS 7.0 (or later version) compatible device</span></span>
* <span data-ttu-id="fdaa3-118">iOS fejlesztőprogrambeli tagság</span><span class="sxs-lookup"><span data-stu-id="fdaa3-118">iOS Developer Program membership</span></span>
* <span data-ttu-id="fdaa3-119">[Xamarin Studio]</span><span class="sxs-lookup"><span data-stu-id="fdaa3-119">[Xamarin Studio]</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="fdaa3-120">Az iOS leküldéses értesítések konfigurációs követelményei miatt kell telepítenie és hello mintaalkalmazás egy fizikai iOS-eszközön (iPhone vagy iPad) tesztelje hello szimulátor helyett.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-120">Because of configuration requirements for iOS push notifications, you must deploy and test hello sample application on a physical iOS device (iPhone or iPad) instead of in hello simulator.</span></span>
  > 
  > 

<span data-ttu-id="fdaa3-121">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Xamarin iOS-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-121">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin iOS apps.</span></span>

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="fdaa3-122">Az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdaa3-122">Configure your notification hub</span></span>
<span data-ttu-id="fdaa3-123">Ez a szakasz végigvezeti egy új értesítési központ létrehozása és konfigurálása a hitelesítés az apns-sel hello segítségével **.p12** létrehozott leküldéses tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-123">This section walks you through creating a new notification hub and configuring authentication with APNS using hello **.p12** push certificate that you created.</span></span> <span data-ttu-id="fdaa3-124">Ha azt szeretné, hogy toouse egy már létrehozott értesítési központot, kihagyhatja toostep 5.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-124">If you want toouse a notification hub that you have already created, you can skip toostep 5.</span></span>

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p><span data-ttu-id="fdaa3-125">Tooconfigure hello APNS-kapcsolatot az Azure-portálon hello szeretnénk, nyissa meg az értesítési központ beállításait, ande kattintson azon <b>értesítési szolgáltatások</b>, és kattintson a hello <b>Apple (APNS)</b> hello lista elemére.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-125">As we want tooconfigure hello APNS connection, in hello Azure Portal, open your Notification Hub settings, ande click on <b>Notification Services</b>, and then click hello <b>Apple (APNS)</b> item in hello list.</span></span> <span data-ttu-id="fdaa3-126">Ha végzett, kattintson a <b>tanúsítvány feltöltése</b> és select hello <b>.p12</b> korábbi, valamint hello tanúsítvány jelszava hello exportált tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-126">Once done, click on <b>Upload Certificate</b> and select hello <b>.p12</b> certificate that you exported earlier, as well as hello password for hello certificate.</span></span></p>

<p><span data-ttu-id="fdaa3-127">Győződjön meg arról, hogy tooselect <b>védőfal</b> módot, mivel küldi leküldéses üzenetek fejlesztői környezetben.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-127">Make sure tooselect <b>Sandbox</b> mode since you will be sending push messages in a development environment.</span></span> <span data-ttu-id="fdaa3-128">Csak a hello használata <b>éles</b> beállítást, ha azt szeretné, hogy toosend leküldéses értesítések toousers akik már megvásárolták az alkalmazást hello áruházból.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-128">Only use hello <b>Production</b> setting if you want toosend push notifications toousers who already purchased your app from hello store.</span></span></p>
</li>
</ol>
<span data-ttu-id="fdaa3-129">&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)</span><span class="sxs-lookup"><span data-stu-id="fdaa3-129">&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)</span></span>

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

<span data-ttu-id="fdaa3-130">Az értesítési központ most már konfigurált toowork az apns-sel, és rendelkezik hello kapcsolati karakterláncok tooregister az alkalmazást, és leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-130">Your notification hub is now configured toowork with APNS, and you have hello connection strings tooregister your app and send push notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="fdaa3-131">Csatlakozás az alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="fdaa3-131">Connect your app toohello notification hub</span></span>
#### <a name="create-a-new-project"></a><span data-ttu-id="fdaa3-132">Új projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdaa3-132">Create a new project</span></span>
1. <span data-ttu-id="fdaa3-133">A Xamarin Studióban hozzon létre egy új iOS-projektet, és válassza ki a hello **egyesített API** > **egyetlen nézetben alkalmazás** sablont.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-133">In Xamarin Studio, create a new iOS project and select hello **Unified API** > **Single View Application** template.</span></span>
   
     ![Xamarin Studio – Alkalmazástípus kiválasztása][31]
2. <span data-ttu-id="fdaa3-135">Hivatkozás toohello Azure Messaging összetevő hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-135">Add a reference toohello Azure Messaging component.</span></span> <span data-ttu-id="fdaa3-136">A hello megoldás nézetet, kattintson a jobb gombbal hello **összetevők** mappa a projekthez, és válassza a **további összetevők beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-136">In hello Solution view, right-click hello **Components** folder for your project and choose **Get More Components**.</span></span> <span data-ttu-id="fdaa3-137">Keresse meg a hello **Azure Messaging** összetevő, és adja hozzá a hello összetevő tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-137">Search for hello **Azure Messaging** component and add hello component tooyour project.</span></span>
3. <span data-ttu-id="fdaa3-138">A **AppDelegate.cs**, adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-138">In **AppDelegate.cs**, add hello following using statement:</span></span>
   
        using WindowsAzure.Messaging;
4. <span data-ttu-id="fdaa3-139">Deklarálja az **SBNotificationHub** egy példányát:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-139">Declare an instance of **SBNotificationHub**:</span></span>
   
        private SBNotificationHub Hub { get; set; }
5. <span data-ttu-id="fdaa3-140">Hozzon létre egy **Constants.cs** hello változók a következő osztályra:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-140">Create a **Constants.cs** class with hello following variables:</span></span>
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. <span data-ttu-id="fdaa3-141">A **AppDelegate.cs**, frissítése **FinishedLaunching()** toomatch hello következő:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-141">In **AppDelegate.cs**, update **FinishedLaunching()** toomatch hello following:</span></span>
   
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
7. <span data-ttu-id="fdaa3-142">Bírálja felül a hello **RegisteredForRemoteNotifications()** metódus a **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-142">Override hello **RegisteredForRemoteNotifications()** method in **AppDelegate.cs**:</span></span>
   
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
8. <span data-ttu-id="fdaa3-143">Bírálja felül a hello **ReceivedRemoteNotification()** metódus a **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-143">Override hello **ReceivedRemoteNotification()** method in **AppDelegate.cs**:</span></span>
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. <span data-ttu-id="fdaa3-144">Hozza létre a következőket hello **ProcessNotification()** metódus a **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-144">Create hello following **ProcessNotification()** method in **AppDelegate.cs**:</span></span>
   
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
   > <span data-ttu-id="fdaa3-145">Kiválaszthatja a toooverride **FailedToRegisterForRemoteNotifications()** toohandle olyan helyzetekben, például a hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-145">You can choose toooverride **FailedToRegisterForRemoteNotifications()** toohandle situations such as no network connection.</span></span> <span data-ttu-id="fdaa3-146">Ez akkor kifejezetten fontos, ahol hello felhasználói (például repülőgép üzemmódban) kapcsolat nélküli módban is elindíthatják az alkalmazást, és azt szeretné, hogy toohandle leküldéses üzenetküldési forgatókönyveket adott tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-146">This is especially important where hello user might start your application in offline mode (e.g. Airplane) and you want toohandle push messaging scenarios specific tooyour app.</span></span>
   > 
   > 
10. <span data-ttu-id="fdaa3-147">Hello alkalmazás futtatható az eszközön.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-147">Run hello app on your device.</span></span>

## <a name="sending-push-notifications"></a><span data-ttu-id="fdaa3-148">Leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="fdaa3-148">Sending Push Notifications</span></span>
<span data-ttu-id="fdaa3-149">Leküldéses értesítések fogadásának az alkalmazásban a hello küldött értesítésekkel tesztelheti [Azure Portal] keresztül hello **küldés tesztelése** hello képesség **hibaelhárítás** eszközkészlet, közvetlenül hello értesítési központ lapján, ahogy az alábbi üdvözlő képernyőt.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-149">You can test receiving push notifications in your app by sending notifications in hello [Azure Portal] via hello **Test Send** capability in hello **Troubleshooting** toolset, right in hello notification hub page, as shown in hello screen below.</span></span>

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

<span data-ttu-id="fdaa3-150">A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-150">Push notifications are normally sent through a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="fdaa3-151">Hello REST API-t használhatja közvetlen toosend leküldéses üzenetek, ha a szalagtár nem érhető el a forgatókönyvben is.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-151">You can also use hello REST API directly toosend push messages if a library is not available in your scenario.</span></span> 

<span data-ttu-id="fdaa3-152">Ebben az oktatóanyagban a rendszer egyszerűen és csak az ügyfélalkalmazás tesztelését küldött értesítésekkel hello .NET SDK használatával egy háttér-szolgáltatás helyett egy konzolalkalmazás értesítési központjának bemutatása.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-152">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="fdaa3-153">Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyagot hello következő lépés az értesítéseknek ASP.NET-háttérrendszerből történő küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-153">We recommend hello [Use Notification Hubs toopush notifications toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="fdaa3-154">A következő módszerekkel hello azonban az értesítések küldésével használhatók:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-154">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="fdaa3-155">**REST-felület**: bármely háttér platformon hello segítségével támogathatja a leküldéses értesítési [REST-felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="fdaa3-155">**REST Interface**:  You can support push notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="fdaa3-156">**A Microsoft Azure Notification Hubs .NET SDK**: hello Nuget-Csomagkezelőt a Visual Studio, a futtasson [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="fdaa3-156">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="fdaa3-157">**NODE.js** : [hogyan toouse Notification Hubs Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="fdaa3-157">**Node.js** : [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>

<span data-ttu-id="fdaa3-158">**Mobile Apps**: A példa bemutatja, hogyan toosend értesítések a Notification Hubs szolgáltatással integrált Azure App Service Mobile Apps-háttérrendszerből lásd [Hozzáadás leküldéses értesítések tooyour mobilalkalmazás](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="fdaa3-158">**Mobile Apps**: For an example of how toosend notifications from an Azure App Service Mobile Apps backend that's integrated with Notification Hubs, see [Add push notifications tooyour mobile app](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>

* <span data-ttu-id="fdaa3-159">**Java / PHP**: hogyan toosend leküldéses értesítések segítségével hello REST API-k példát lásd: "hogyan toouse Notification Hubs Java/php-ből" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="fdaa3-159">**Java / PHP**: For an example of how toosend push notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

#### <a name="optional-send-push-notifications-from-a-net-console-app"></a><span data-ttu-id="fdaa3-160">(Választható) Leküldéses értesítések küldése .NET-konzolalkalmazásból</span><span class="sxs-lookup"><span data-stu-id="fdaa3-160">(Optional) Send Push Notifications from a .NET Console App</span></span>
<span data-ttu-id="fdaa3-161">Ebben a szakaszban egy egyszerű .NET konzolalkalmazás használatával küldünk leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-161">In this section, we will send push notifications by using a simple .NET console app.</span></span> <span data-ttu-id="fdaa3-162">A jelen példában hello célokra átváltunk tooa Windows fejlesztői környezetre, a Visual Studio már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-162">For hello purposes of this example, we will switch tooa Windows development environment that has Visual Studio already installed.</span></span>

1. <span data-ttu-id="fdaa3-163">A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-163">In Visual Studio, create a new Visual C# console application:</span></span>
   
       ![Visual Studio - Create a new console application][213]
2. <span data-ttu-id="fdaa3-164">A Visual Studióban kattintson az **Eszközök**, a **NuGet Package Manager** (NuGet-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-164">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="fdaa3-165">a Visual Studio munkaterületének aljához rögzített toohello hello Csomagkezelő konzolján jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-165">hello package manager console should appear docked toohello bottom of your Visual Studio workspace.</span></span>
3. <span data-ttu-id="fdaa3-166">Hello Package Manager Console ablakban, állítson be hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-166">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="fdaa3-167">Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-167">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="fdaa3-168">Nyissa meg hello `Program.cs` fájlt, és adja hozzá a következő hello `using` utasítást, amely biztosítja, hogy használhatunk Azure-osztályokat és -függvényeket a fő osztályban:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-168">Open hello `Program.cs` file and add hello following `using` statement, ensuring that we can use Azure classes and functions within your main class:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="fdaa3-169">Az a `Program` osztály, adja hozzá a következő metódus hello (ne feledje tooreplace hello **kapcsolati karakterlánc** és **központnév**):</span><span class="sxs-lookup"><span data-stu-id="fdaa3-169">In your `Program` class, add hello following method (don't forget tooreplace hello **connection string** and **hub name**):</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. <span data-ttu-id="fdaa3-170">Adja hozzá az alábbi hello a `Main` módszert:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-170">Add hello following lines in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="fdaa3-171">Nyomja le az hello F5 kulcs toorun hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-171">Press hello F5 key toorun hello app.</span></span> <span data-ttu-id="fdaa3-172">Néhány másodpercen belül meg kell jelennie egy leküldéses értesítésnek az eszközön.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-172">Within seconds, you should see a push notification appear on your device.</span></span> <span data-ttu-id="fdaa3-173">Függetlenül attól, hogy Wi-Fi- vagy mobil adatátviteli hálózathoz csatlakozik, győződjön meg arról, hogy rendelkezik-e aktív internetkapcsolat hello eszközön.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-173">Whether you are using Wi-Fi or a cellular data network, make sure that you have an active internet connection on hello device.</span></span>

<span data-ttu-id="fdaa3-174">Minden hello lehetséges hasznos adatot megtalálja az hello Apple [helyi és leküldéses értesítések programozásával foglalkozó útmutatójában].</span><span class="sxs-lookup"><span data-stu-id="fdaa3-174">You can find all hello possible payloads in hello Apple [Local and Push Notification Programming Guide].</span></span>

#### <a name="optional-send-notifications-from-a-mobile-service"></a><span data-ttu-id="fdaa3-175">(Választható) Értesítések küldése mobilszolgáltatásból</span><span class="sxs-lookup"><span data-stu-id="fdaa3-175">(Optional) Send Notifications from a Mobile Service</span></span>
<span data-ttu-id="fdaa3-176">Ebben a szakaszban egy mobilszolgáltatás használatával küldünk leküldéses értesítéseket egy csomópontparancsfájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-176">In this section, we will send push notifications using a mobile service through a node script.</span></span>

<span data-ttu-id="fdaa3-177">hajtsa végre a toosend egy mobilszolgáltatás használatával értesítést [Ismerkedés a Mobile Services], majd:</span><span class="sxs-lookup"><span data-stu-id="fdaa3-177">toosend a notification by using a mobile service, follow [Get started with Mobile Services], and then:</span></span>

1. <span data-ttu-id="fdaa3-178">Jelentkezzen be toohello [klasszikus Azure portál], és jelölje ki a mobilszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-178">Sign in toohello [Azure Classic Portal], and select your mobile service.</span></span>
2. <span data-ttu-id="fdaa3-179">Jelölje be hello **Feladatütemező** hello legfelső lap.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-179">Select hello **Scheduler** tab on hello top.</span></span>
   
       ![Azure Classic Portal - Scheduler][215]
3. <span data-ttu-id="fdaa3-180">Hozzon létre egy új ütemezett feladatot, szúrjon be egy nevet, és válassza az **On demand** (Igény szerint) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-180">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
       ![Azure Classic Portal - Create new job][216]
4. <span data-ttu-id="fdaa3-181">Ha hello feladat jön létre, kattintson a hello feladat neve.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-181">When hello job is created, click hello job name.</span></span> <span data-ttu-id="fdaa3-182">Kattintson a hello **parancsfájl** hello felső sávon fülre.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-182">Then click hello **Script** tab on hello top bar.</span></span>
5. <span data-ttu-id="fdaa3-183">Helyezze be a következő parancsfájlt a scheduler függvényébe hello.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-183">Insert hello following script inside your scheduler function.</span></span> <span data-ttu-id="fdaa3-184">Győződjön meg arról, hogy tooreplace hello helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultFullSharedAccessSignature* korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-184">Make sure tooreplace hello placeholders with your notification hub name and hello connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="fdaa3-185">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-185">Click **Save**.</span></span>
   
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
6. <span data-ttu-id="fdaa3-186">Kattintson a **egyszeri futtatás** hello alsó sáv.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-186">Click **Run Once** on hello bottom bar.</span></span> <span data-ttu-id="fdaa3-187">Az eszköznek egy riasztást kell fogadnia.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-187">You should receive an alert on your device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdaa3-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdaa3-188">Next steps</span></span>
<span data-ttu-id="fdaa3-189">Ez az egyszerű példában küldött leküldéses értesítések tooall az iOS-eszközökre.</span><span class="sxs-lookup"><span data-stu-id="fdaa3-189">In this simple example, you broadcasted push notifications tooall your iOS devices.</span></span> <span data-ttu-id="fdaa3-190">A rendezés tootarget adott felhasználók, tekintse meg a toohello oktatóanyag [Notification Hubs használata toopush értesítések toousers].</span><span class="sxs-lookup"><span data-stu-id="fdaa3-190">In order tootarget specific users, refer toohello tutorial [Use Notification Hubs toopush notifications toousers].</span></span> <span data-ttu-id="fdaa3-191">Ha azt szeretné, toosegment a felhasználókat érdeklődési körök, olvasható [legfrissebb hírek Notification Hubs használata toosend].</span><span class="sxs-lookup"><span data-stu-id="fdaa3-191">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="fdaa3-192">További tudnivalók toouse értesítési központok [Notification Hubs használatával] és hello [Notification Hubs útmutató-toofor iOS].</span><span class="sxs-lookup"><span data-stu-id="fdaa3-192">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance] and in hello [Notification Hubs How-toofor iOS].</span></span>

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
