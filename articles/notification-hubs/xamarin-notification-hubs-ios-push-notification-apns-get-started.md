---
title: "iOS leküldéses értesítések küldése a Notification Hubs használatával Xamarin-alkalmazásokba | Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogy hogyan használható az Azure Notification Hubs leküldéses értesítések küldésére Xamarin iOS-alkalmazásokba."
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
ms.openlocfilehash: 72a81fa0deb34ace77b8fb9b1a4e6b24ee164b35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a><span data-ttu-id="1dcb4-104">iOS leküldéses értesítések küldése a Notification Hubs használatával Xamarin-alkalmazásokba</span><span class="sxs-lookup"><span data-stu-id="1dcb4-104">iOS Push Notifications with Notification Hubs for Xamarin apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="1dcb4-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1dcb4-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1dcb4-106">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-106">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="1dcb4-107">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="1dcb4-108">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="1dcb4-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="1dcb4-109">Ez az oktatóanyag azt mutatja be, hogy hogyan használható az Azure Notification Hubs leküldéses értesítések küldésére iOS-alkalmazásokba.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-109">This tutorial shows you how to use Azure Notification Hubs to send push notifications to an iOS application.</span></span>
<span data-ttu-id="1dcb4-110">Létre fog hozni egy üres Xamarin.iOS-alkalmazást, amely leküldéses értesítéseket fogad az [Apple Push Notification szolgáltatás (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) használatával.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-110">You'll create a blank Xamarin.iOS app that receives push notifications by using the [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).</span></span> <span data-ttu-id="1dcb4-111">Amikor végzett, képes lesz az értesítési központ használatával leküldéses értesítéseket küldeni az alkalmazást futtató összes eszközre.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-111">When you're finished, you'll be able to use your notification hub to broadcast push notifications to all the devices running your app.</span></span> <span data-ttu-id="1dcb4-112">A befejezett kód a minta [NotificationHubs alkalmazásban][GitHub] érhető el.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-112">The finished code is available in the [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="1dcb4-113">Ez az oktatóanyag az egyszerű leküldéses üzenetküldési forgatókönyvet mutatja be a Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-113">This tutorial demonstrates the simple push message broadcast scenario with Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1dcb4-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1dcb4-114">Prerequisites</span></span>
<span data-ttu-id="1dcb4-115">Az oktatóanyaghoz az alábbiakra lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-115">This tutorial requires the following:</span></span>

* <span data-ttu-id="1dcb4-116">[XCode 6.0][Install Xcode]</span><span class="sxs-lookup"><span data-stu-id="1dcb4-116">[Xcode 6.0][Install Xcode]</span></span>
* <span data-ttu-id="1dcb4-117">Az iOS 7.0-tel (vagy újabb verzióval) kompatibilis eszköz</span><span class="sxs-lookup"><span data-stu-id="1dcb4-117">An iOS 7.0 (or later version) compatible device</span></span>
* <span data-ttu-id="1dcb4-118">iOS fejlesztőprogrambeli tagság</span><span class="sxs-lookup"><span data-stu-id="1dcb4-118">iOS Developer Program membership</span></span>
* <span data-ttu-id="1dcb4-119">[Xamarin Studio]</span><span class="sxs-lookup"><span data-stu-id="1dcb4-119">[Xamarin Studio]</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="1dcb4-120">Az iOS leküldéses értesítések konfigurációs követelményei miatt a mintaalkalmazást a szimulátor helyett egy fizikai iOS-eszközön (iPhone vagy iPad) kell üzembe helyeznie és tesztelnie.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-120">Because of configuration requirements for iOS push notifications, you must deploy and test the sample application on a physical iOS device (iPhone or iPad) instead of in the simulator.</span></span>
  > 
  > 

<span data-ttu-id="1dcb4-121">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Xamarin iOS-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-121">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin iOS apps.</span></span>

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="1dcb4-122">Az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1dcb4-122">Configure your notification hub</span></span>
<span data-ttu-id="1dcb4-123">Ez a szakasz végigvezeti egy új értesítési központ létrehozásának és az APNS-hitelesítés konfigurálásának folyamatán a létrehozott **.p12** leküldéses tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-123">This section walks you through creating a new notification hub and configuring authentication with APNS using the **.p12** push certificate that you created.</span></span> <span data-ttu-id="1dcb4-124">Ha egy már korábban létrehozott értesítési központot kíván használni, egyből az 5. lépésre ugorhat.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-124">If you want to use a notification hub that you have already created, you can skip to step 5.</span></span>

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p><span data-ttu-id="1dcb4-125">Mivel az APNS-kapcsolatot szeretnénk konfigurálni, nyissa meg az értesítési központ beállításait az Azure Portalon, kattintson a <b>Notification Services</b> (Értesítési szolgáltatások) lehetőségre, majd az <b>Apple (APNS)</b> elemre a listában.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-125">As we want to configure the APNS connection, in the Azure Portal, open your Notification Hub settings, ande click on <b>Notification Services</b>, and then click the <b>Apple (APNS)</b> item in the list.</span></span> <span data-ttu-id="1dcb4-126">Amint elkészült, kattintson az <b>Upload Certificate</b> (Tanúsítvány feltöltése) parancsra, és jelölje ki a korábban exportált <b>.p12</b> tanúsítványt, valamint adja meg annak jelszavát.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-126">Once done, click on <b>Upload Certificate</b> and select the <b>.p12</b> certificate that you exported earlier, as well as the password for the certificate.</span></span></p>

<p><span data-ttu-id="1dcb4-127">Válassza a <b>Sandbox</b> (Védőfal) módot, mivel fejlesztői környezetben fog leküldéses üzeneteket küldeni.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-127">Make sure to select <b>Sandbox</b> mode since you will be sending push messages in a development environment.</span></span> <span data-ttu-id="1dcb4-128">A <b>Production</b> (Éles) beállítást kizárólag akkor használja, ha olyan felhasználóknak szeretne leküldéses értesítéseket küldeni, akik már megvásárolták az alkalmazást az áruházból.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-128">Only use the <b>Production</b> setting if you want to send push notifications to users who already purchased your app from the store.</span></span></p>
</li>
</ol>
<span data-ttu-id="1dcb4-129">&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)</span><span class="sxs-lookup"><span data-stu-id="1dcb4-129">&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)</span></span>

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

<span data-ttu-id="1dcb4-130">Az értesítési központ konfigurálva lett az APNS-sel való együttműködésre, és rendelkezik a kapcsolati karakterláncokkal az alkalmazás regisztrálásához és leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-130">Your notification hub is now configured to work with APNS, and you have the connection strings to register your app and send push notifications.</span></span>

## <a name="connect-your-app-to-the-notification-hub"></a><span data-ttu-id="1dcb4-131">Az alkalmazás csatlakoztatása az értesítési központhoz</span><span class="sxs-lookup"><span data-stu-id="1dcb4-131">Connect your app to the notification hub</span></span>
#### <a name="create-a-new-project"></a><span data-ttu-id="1dcb4-132">Új projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="1dcb4-132">Create a new project</span></span>
1. <span data-ttu-id="1dcb4-133">A Xamarin Studióban hozzon létre egy új iOS-projektet, és válassza a **Unified API** > **Single View Application** (Egyesített API > Egynézetes alkalmazás) sablont.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-133">In Xamarin Studio, create a new iOS project and select the **Unified API** > **Single View Application** template.</span></span>
   
     ![Xamarin Studio – Alkalmazástípus kiválasztása][31]
2. <span data-ttu-id="1dcb4-135">Adja hozzá az Azure Messaging összetevőre mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-135">Add a reference to the Azure Messaging component.</span></span> <span data-ttu-id="1dcb4-136">A Solution (Megoldás) nézetben kattintson a jobb gombbal a projekt **Components** (Összetevők) mappájára, és válassza a **Get More Components** (További összetevők beszerzése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-136">In the Solution view, right-click the **Components** folder for your project and choose **Get More Components**.</span></span> <span data-ttu-id="1dcb4-137">Keresse meg az **Azure Messaging** összetevőt, és adja hozzá a projekthez.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-137">Search for the **Azure Messaging** component and add the component to your project.</span></span>
3. <span data-ttu-id="1dcb4-138">Az **AppDelegate.cs** osztályban adja hozzá a következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-138">In **AppDelegate.cs**, add the following using statement:</span></span>
   
        using WindowsAzure.Messaging;
4. <span data-ttu-id="1dcb4-139">Deklarálja az **SBNotificationHub** egy példányát:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-139">Declare an instance of **SBNotificationHub**:</span></span>
   
        private SBNotificationHub Hub { get; set; }
5. <span data-ttu-id="1dcb4-140">Hozza létre a **Constants.cs** osztályt a következő változókkal:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-140">Create a **Constants.cs** class with the following variables:</span></span>
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. <span data-ttu-id="1dcb4-141">Az **AppDelegate.cs** fájlban szerkessze a **FinishedLaunching()** metódust úgy, hogy az egyezzen az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-141">In **AppDelegate.cs**, update **FinishedLaunching()** to match the following:</span></span>
   
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
7. <span data-ttu-id="1dcb4-142">Bírálja felül a **RegisteredForRemoteNotifications()** metódust az **AppDelegate.cs** osztályban:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-142">Override the **RegisteredForRemoteNotifications()** method in **AppDelegate.cs**:</span></span>
   
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
8. <span data-ttu-id="1dcb4-143">Bírálja felül a **ReceivedRemoteNotification()** metódust az **AppDelegate.cs** osztályban:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-143">Override the **ReceivedRemoteNotification()** method in **AppDelegate.cs**:</span></span>
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. <span data-ttu-id="1dcb4-144">Hozza létre a következő **ProcessNotification()** metódust az **AppDelegate.cs** osztályban:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-144">Create the following **ProcessNotification()** method in **AppDelegate.cs**:</span></span>
   
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
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
   > <span data-ttu-id="1dcb4-145">Dönthet úgy, hogy felülbírálja a **FailedToRegisterForRemoteNotifications()** metódust az olyan helyzetek kezelésére, amikor például nem áll rendelkezésre hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-145">You can choose to override **FailedToRegisterForRemoteNotifications()** to handle situations such as no network connection.</span></span> <span data-ttu-id="1dcb4-146">Ez különösen fontos, ha a felhasználók offline módban is elindíthatják az alkalmazást (például repülőgép üzemmódban), és kezelni szeretné az alkalmazással kapcsolatos egyedi leküldéses üzenetküldési forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-146">This is especially important where the user might start your application in offline mode (e.g. Airplane) and you want to handle push messaging scenarios specific to your app.</span></span>
   > 
   > 
10. <span data-ttu-id="1dcb4-147">Futtassa az alkalmazást az eszközön.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-147">Run the app on your device.</span></span>

## <a name="sending-push-notifications"></a><span data-ttu-id="1dcb4-148">Leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="1dcb4-148">Sending Push Notifications</span></span>
<span data-ttu-id="1dcb4-149">A leküldéses értesítések fogadásának az alkalmazásban való teszteléséhez értesítéseket küldhet az [Azure Portal] közvetlenül az értesítési központ lapján található **Hibaelhárítás** eszközkészlet **Küldés tesztelése** funkciója használatával, az alábbi képernyőn látható módon.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-149">You can test receiving push notifications in your app by sending notifications in the [Azure Portal] via the **Test Send** capability in the **Troubleshooting** toolset, right in the notification hub page, as shown in the screen below.</span></span>

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

<span data-ttu-id="1dcb4-150">A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-150">Push notifications are normally sent through a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="1dcb4-151">A leküldéses üzenetek küldéséhez használhatja közvetlenül a REST API-t is, ha a forgatókönyvben nem érhető el kódtár.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-151">You can also use the REST API directly to send push messages if a library is not available in your scenario.</span></span> 

<span data-ttu-id="1dcb4-152">Ebben az oktatóanyagban az egyszerűbb megoldást választjuk, és az ügyfélalkalmazás tesztelését egy konzolalkalmazás értesítési központjának .NET SDK-ja használatával küldött értesítésekkel mutatjuk be háttérszolgáltatás használata helyett.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-152">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using the .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="1dcb4-153">Az értesítéseknek ASP.NET-háttérrendszerből történő küldéséhez következő lépésként [A Notification Hubs használata leküldéses értesítések küldéséhez felhasználók számára](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) oktatóanyagot ajánljuk.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-153">We recommend the [Use Notification Hubs to push notifications to users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial as the next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="1dcb4-154">Értesítések küldéséhez azonban az alábbi megközelítések is alkalmazhatók:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-154">However, the following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="1dcb4-155">**REST-felület**: A [REST-felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) használatával bármilyen háttérplatformon támogathatja a leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-155">**REST Interface**:  You can support push notification on any backend platform using  the [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="1dcb4-156">**Microsoft Azure Notification Hubs .NET SDK**: A Visual Studio NuGet-csomagkezelőjében futtassa a következő parancsot: [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="1dcb4-156">**Microsoft Azure Notification Hubs .NET SDK**: In the Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="1dcb4-157">**Node.js**: [A Notification Hub használata a Node.js-ből](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="1dcb4-157">**Node.js** : [How to use Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>

<span data-ttu-id="1dcb4-158">**Mobile Apps**: A [Leküldéses értesítések hozzáadása Mobile Apps-alkalmazáshoz](../app-service-mobile/app-service-mobile-ios-get-started-push.md) témakörben találhat példát arra, hogy hogyan küldhetők értesítések a Notification Hubs szolgáltatással integrált Azure App Service Mobile Apps háttéralkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-158">**Mobile Apps**: For an example of how to send notifications from an Azure App Service Mobile Apps backend that's integrated with Notification Hubs, see [Add push notifications to your mobile app](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>

* <span data-ttu-id="1dcb4-159">**Java/PHP**: „A Notification Hubs használata Javából/PHP-ből” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)) témakörben találhat példát arra, hogyan küldhetők leküldéses értesítések a REST API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-159">**Java / PHP**: For an example of how to send push notifications by using the REST APIs, see "How to use Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

#### <a name="optional-send-push-notifications-from-a-net-console-app"></a><span data-ttu-id="1dcb4-160">(Választható) Leküldéses értesítések küldése .NET-konzolalkalmazásból</span><span class="sxs-lookup"><span data-stu-id="1dcb4-160">(Optional) Send Push Notifications from a .NET Console App</span></span>
<span data-ttu-id="1dcb4-161">Ebben a szakaszban egy egyszerű .NET konzolalkalmazás használatával küldünk leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-161">In this section, we will send push notifications by using a simple .NET console app.</span></span> <span data-ttu-id="1dcb4-162">A jelen példában átváltunk Windows fejlesztői környezetre, amelyben a Visual Studio már telepítve lett.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-162">For the purposes of this example, we will switch to a Windows development environment that has Visual Studio already installed.</span></span>

1. <span data-ttu-id="1dcb4-163">A Visual Studióban hozzon létre egy új Visual C#-konzolalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-163">In Visual Studio, create a new Visual C# console application:</span></span>
   
       ![Visual Studio - Create a new console application][213]
2. <span data-ttu-id="1dcb4-164">A Visual Studióban kattintson az **Eszközök**, a **NuGet Package Manager** (NuGet-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-164">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="1dcb4-165">A csomagkezelő konzolnak a Visual Studio munkaterületének aljához rögzítve kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-165">The package manager console should appear docked to the bottom of your Visual Studio workspace.</span></span>
3. <span data-ttu-id="1dcb4-166">A Package Manager Console (Csomagkezelő konzol) ablakban az **Alapértelmezett projekt** értékeként adja meg az új konzolalkalmazás-projektet, majd a konzolablakban hajtsa végre az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-166">In the Package Manager Console window, set the **Default project** to your new console application project, and then in the console window, execute the following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="1dcb4-167">Ezzel hozzáad a <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomagot</a> használó Azure Notification Hubs SDK-ra mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-167">This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="1dcb4-168">Nyissa meg a `Program.cs` fájlt, majd adja hozzá a következő `using` utasítást, amely biztosítja, hogy használhatunk Azure-osztályokat és -függvényeket a fő osztályban:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-168">Open the `Program.cs` file and add the following `using` statement, ensuring that we can use Azure classes and functions within your main class:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="1dcb4-169">A `Program` osztályban adja hozzá a következő metódust (ne felejtse el lecserélni a **connection string** (kapcsolati karakterlánc) és a **hub name** (központ neve) helyőrzőt):</span><span class="sxs-lookup"><span data-stu-id="1dcb4-169">In your `Program` class, add the following method (don't forget to replace the **connection string** and **hub name**):</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. <span data-ttu-id="1dcb4-170">Adja hozzá a következő sorokat a `Main` metódushoz:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-170">Add the following lines in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="1dcb4-171">Nyomja le az F5 billentyűt az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-171">Press the F5 key to run the app.</span></span> <span data-ttu-id="1dcb4-172">Néhány másodpercen belül meg kell jelennie egy leküldéses értesítésnek az eszközön.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-172">Within seconds, you should see a push notification appear on your device.</span></span> <span data-ttu-id="1dcb4-173">Függetlenül attól, hogy Wi-Fi- vagy mobil adatátviteli hálózathoz csatlakozik, ellenőrizze, hogy van-e aktív internetkapcsolat az eszközön.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-173">Whether you are using Wi-Fi or a cellular data network, make sure that you have an active internet connection on the device.</span></span>

<span data-ttu-id="1dcb4-174">Az összes lehetséges hasznos adatot megtalálja az Apple [helyi és leküldéses értesítések programozásával foglalkozó útmutatójában].</span><span class="sxs-lookup"><span data-stu-id="1dcb4-174">You can find all the possible payloads in the Apple [Local and Push Notification Programming Guide].</span></span>

#### <a name="optional-send-notifications-from-a-mobile-service"></a><span data-ttu-id="1dcb4-175">(Választható) Értesítések küldése mobilszolgáltatásból</span><span class="sxs-lookup"><span data-stu-id="1dcb4-175">(Optional) Send Notifications from a Mobile Service</span></span>
<span data-ttu-id="1dcb4-176">Ebben a szakaszban egy mobilszolgáltatás használatával küldünk leküldéses értesítéseket egy csomópontparancsfájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-176">In this section, we will send push notifications using a mobile service through a node script.</span></span>

<span data-ttu-id="1dcb4-177">Ha mobilszolgáltatással kíván értesítést küldeni, kövesse [A Mobile Services használatának első lépéseit] című témakör utasításait, majd:</span><span class="sxs-lookup"><span data-stu-id="1dcb4-177">To send a notification by using a mobile service, follow [Get started with Mobile Services], and then:</span></span>

1. <span data-ttu-id="1dcb4-178">Jelentkezzen be a [klasszikus Azure portál], majd jelölje ki a mobilszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-178">Sign in to the [Azure Classic Portal], and select your mobile service.</span></span>
2. <span data-ttu-id="1dcb4-179">Válassza az oldal tetején található **Scheduler** fület.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-179">Select the **Scheduler** tab on the top.</span></span>
   
       ![Azure Classic Portal - Scheduler][215]
3. <span data-ttu-id="1dcb4-180">Hozzon létre egy új ütemezett feladatot, szúrjon be egy nevet, és válassza az **On demand** (Igény szerint) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-180">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
       ![Azure Classic Portal - Create new job][216]
4. <span data-ttu-id="1dcb4-181">A feladat létrehozását követően kattintson a feladat nevére.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-181">When the job is created, click the job name.</span></span> <span data-ttu-id="1dcb4-182">Ezután válassza a felső sávon található **Script** (Parancsfájl) fület.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-182">Then click the **Script** tab on the top bar.</span></span>
5. <span data-ttu-id="1dcb4-183">Szúrja be a következő parancsfájlt a Scheduler függvényébe.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-183">Insert the following script inside your scheduler function.</span></span> <span data-ttu-id="1dcb4-184">Cserélje le a helyőrzőket az értesítési központ nevére és a *DefaultFullSharedAccessSignature* kapcsolati karakterláncra, amelyet korábban szerzett be.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-184">Make sure to replace the placeholders with your notification hub name and the connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="1dcb4-185">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-185">Click **Save**.</span></span>
   
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
6. <span data-ttu-id="1dcb4-186">Kattintson az alsó sáv **Run Once** (Futtatás egyszer) parancsára.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-186">Click **Run Once** on the bottom bar.</span></span> <span data-ttu-id="1dcb4-187">Az eszköznek egy riasztást kell fogadnia.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-187">You should receive an alert on your device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1dcb4-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1dcb4-188">Next steps</span></span>
<span data-ttu-id="1dcb4-189">Ebben az egyszerű példában leküldéses értesítéseket küldött az összes iOS-eszközre.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-189">In this simple example, you broadcasted push notifications to all your iOS devices.</span></span> <span data-ttu-id="1dcb4-190">Ha adott felhasználóknak szeretne értesítést küldeni, tekintse meg [A Notification Hubs használata leküldéses értesítések küldéséhez felhasználók számára] oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-190">In order to target specific users, refer to the tutorial [Use Notification Hubs to push notifications to users].</span></span> <span data-ttu-id="1dcb4-191">Ha a felhasználókat érdeklődési körök alapján szeretné szegmentálni, olvassa el a [Use Notification Hubs to send breaking news] (Friss hírek küldése Notification Hubs használatával) című témakört.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-191">If you want to segment your users by interest groups, you can read [Use Notification Hubs to send breaking news].</span></span> <span data-ttu-id="1dcb4-192">A Notification Hubs használatával kapcsolatban a [Notification Hubs használatával] és [Notification Hubs iOS rendszeren való használatával] foglalkozó témakörben tekinthet meg további információt.</span><span class="sxs-lookup"><span data-stu-id="1dcb4-192">Learn more about how to use Notification Hubs in [Notification Hubs Guidance] and in the [Notification Hubs How-To for iOS].</span></span>

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

<span data-ttu-id="1dcb4-193">[A Mobile Services használatának első lépéseit]: /develop/mobile/tutorials/get-started-xamarin-ios</span><span class="sxs-lookup"><span data-stu-id="1dcb4-193">[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios</span></span>
<span data-ttu-id="1dcb4-194">[klasszikus Azure portál]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="1dcb4-194">[Azure Classic Portal]: https://manage.windowsazure.com/</span></span>
<span data-ttu-id="1dcb4-195">[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx</span><span class="sxs-lookup"><span data-stu-id="1dcb4-195">[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx</span></span>
<span data-ttu-id="1dcb4-196">[Notification Hubs iOS rendszeren való használatával]: http://msdn.microsoft.com/library/jj927168.aspx</span><span class="sxs-lookup"><span data-stu-id="1dcb4-196">[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx</span></span>
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

<span data-ttu-id="1dcb4-197">[A Notification Hubs használata leküldéses értesítések küldéséhez felhasználók számára]: /manage/services/notification-hubs/notify-users-aspnet</span><span class="sxs-lookup"><span data-stu-id="1dcb4-197">[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet</span></span>
<span data-ttu-id="1dcb4-198">[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet</span><span class="sxs-lookup"><span data-stu-id="1dcb4-198">[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet</span></span>

<span data-ttu-id="1dcb4-199">[helyi és leküldéses értesítések programozásával foglalkozó útmutatójában]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1</span><span class="sxs-lookup"><span data-stu-id="1dcb4-199">[Local and Push Notification Programming Guide]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1</span></span>
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
<span data-ttu-id="1dcb4-200">[Xamarin Studio]: http://xamarin.com/download</span><span class="sxs-lookup"><span data-stu-id="1dcb4-200">[Xamarin Studio]: http://xamarin.com/download</span></span>
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
<span data-ttu-id="1dcb4-201">[Azure Portal]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="1dcb4-201">[Azure Portal]: https://portal.azure.com</span></span>
