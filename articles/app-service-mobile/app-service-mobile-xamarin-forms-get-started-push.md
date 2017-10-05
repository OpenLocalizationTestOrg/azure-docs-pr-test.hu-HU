---
title: "Leküldéses értesítések hozzáadása a Xamarin.Forms-alkalmazás |} Microsoft Docs"
description: "Útmutató: Azure-szolgáltatások segítségével több platformra leküldéses értesítések küldéséhez a Xamarin.Forms-alkalmazásokra."
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: 912367636f1b26b3b07fbd5fe3fe8ed053218fd5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a><span data-ttu-id="2680f-103">Leküldéses értesítések hozzáadása a Xamarin.Forms-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2680f-103">Add push notifications to your Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="2680f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2680f-104">Overview</span></span>
<span data-ttu-id="2680f-105">Ebben az oktatóanyagban leküldéses értesítések hozzáadása a projektek eredményeként a [Xamarin.Forms gyors üzembe helyezési](app-service-mobile-xamarin-forms-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2680f-105">In this tutorial, you add push notifications to all the projects that resulted from the [Xamarin.Forms quick start](app-service-mobile-xamarin-forms-get-started.md).</span></span> <span data-ttu-id="2680f-106">Ez azt jelenti, hogy leküldéses értesítés minden platformfüggetlen az ügyfeleknek kiküldött minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="2680f-106">This means that a push notification is sent to all cross-platform clients every time a record is inserted.</span></span>

<span data-ttu-id="2680f-107">Ha nem használja a letöltött gyors üzembe helyezési kiszolgálóprojektet, szüksége lesz a leküldéses értesítési kiterjesztési csomagot.</span><span class="sxs-lookup"><span data-stu-id="2680f-107">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="2680f-108">További információkért lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="2680f-108">For more information, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2680f-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2680f-109">Prerequisites</span></span>
<span data-ttu-id="2680f-110">Az iOS, szüksége lesz egy [Apple Fejlesztőprogrambeli tagság](https://developer.apple.com/programs/ios/) és egy fizikai iOS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="2680f-110">For iOS, you will need an [Apple Developer Program membership](https://developer.apple.com/programs/ios/) and a physical iOS device.</span></span> <span data-ttu-id="2680f-111">A [iOS-szimulátorban nem támogatja a leküldéses értesítések](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="2680f-111">The [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span>

## <span data-ttu-id="2680f-112"><a name="configure-hub"></a>Egy értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2680f-112"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a><span data-ttu-id="2680f-113">Frissítés a kiszolgáló projekt leküldéses értesítések küldéséhez</span><span class="sxs-lookup"><span data-stu-id="2680f-113">Update the server project to send push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a><span data-ttu-id="2680f-114">Konfigurálja és futtassa az Android-projektre (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="2680f-114">Configure and run the Android project (optional)</span></span>
<span data-ttu-id="2680f-115">Ez a szakasz az Android a Xamarin.Forms Droid-projektek a leküldéses értesítések engedélyezéséhez végezze el.</span><span class="sxs-lookup"><span data-stu-id="2680f-115">Complete this section to enable push notifications for the Xamarin.Forms Droid project for Android.</span></span>

### <a name="enable-firebase-cloud-messaging-fcm"></a><span data-ttu-id="2680f-116">Firebase Cloud Messaging (FCM) engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2680f-116">Enable Firebase Cloud Messaging (FCM)</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a><span data-ttu-id="2680f-117">A Mobile Apps háttér leküldéses kérések küldése FCM használatával konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2680f-117">Configure the Mobile Apps back end to send push requests by using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a><span data-ttu-id="2680f-118">Leküldéses értesítések hozzáadása az Android-projekt</span><span class="sxs-lookup"><span data-stu-id="2680f-118">Add push notifications to the Android project</span></span>
<span data-ttu-id="2680f-119">A háttérrendszer működésében FCM konfigurált adhat hozzá összetevők és kódok FCM regisztrálni az ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="2680f-119">With the back end configured with FCM, you can add components and codes to the client to register with FCM.</span></span> <span data-ttu-id="2680f-120">A Mobile Apps háttér keresztül az Azure Notification Hubs leküldéses értesítések regisztrálása, és értesítéseket is.</span><span class="sxs-lookup"><span data-stu-id="2680f-120">You can also register for push notifications with Azure Notification Hubs through the Mobile Apps back end, and receive notifications.</span></span>

1. <span data-ttu-id="2680f-121">Az a **Droid** projektre, kattintson a jobb gombbal a **összetevők** mappára, majd kattintson **több összetevők beolvasása...** .</span><span class="sxs-lookup"><span data-stu-id="2680f-121">In the **Droid** project, right-click the **Components** folder, and click **Get More Components...**.</span></span> <span data-ttu-id="2680f-122">Majd keresse meg a **Google Cloud Messaging Client** összetevő, és adja hozzá a projekthez.</span><span class="sxs-lookup"><span data-stu-id="2680f-122">Then search for the **Google Cloud Messaging Client** component and add it to the project.</span></span> <span data-ttu-id="2680f-123">Ez az összetevő a Xamarin Android-projekt leküldéses értesítések támogat.</span><span class="sxs-lookup"><span data-stu-id="2680f-123">This component supports push notifications for a Xamarin Android project.</span></span>
2. <span data-ttu-id="2680f-124">Nyissa meg a MainActivity.cs projektfájlt, és a fájl elejéhez adja hozzá a következő utasítást:</span><span class="sxs-lookup"><span data-stu-id="2680f-124">Open the MainActivity.cs project file, and add the following statement at the top of the file:</span></span>

        using Gcm.Client;
3. <span data-ttu-id="2680f-125">Adja hozzá a következő kódot a **OnCreate** metódus hívása után **LoadApplication**:</span><span class="sxs-lookup"><span data-stu-id="2680f-125">Add the following code to the **OnCreate** method after the call to **LoadApplication**:</span></span>

        try
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. <span data-ttu-id="2680f-126">Adjon hozzá egy új **CreateAndShowDialog** segítő módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2680f-126">Add a new **CreateAndShowDialog** helper method, as follows:</span></span>

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. <span data-ttu-id="2680f-127">Adja hozzá a következő kódot a **MainActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="2680f-127">Add the following code to the **MainActivity** class:</span></span>

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    <span data-ttu-id="2680f-128">Ez mutatja az aktuális **MainActivity** példány, ezért a fő felhasználói felület szálán végezhetünk.</span><span class="sxs-lookup"><span data-stu-id="2680f-128">This exposes the current **MainActivity** instance, so we can execute on the main UI thread.</span></span>
6. <span data-ttu-id="2680f-129">Inicializálni a `instance` változó elején a **OnCreate** módszert az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="2680f-129">Initialize the `instance` variable at the beginning of the **OnCreate** method, as follows.</span></span>

        // Set the current instance of MainActivity.
        instance = this;
7. <span data-ttu-id="2680f-130">Új osztály fájl hozzáadásához a **Droid** nevű projekt `GcmService.cs`, és győződjön meg arról, hogy a következő **használatával** utasításokat a fájl elején szerepelnek:</span><span class="sxs-lookup"><span data-stu-id="2680f-130">Add a new class file to the **Droid** project named `GcmService.cs`, and make sure the following **using** statements are present at the top of the file:</span></span>

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;
8. <span data-ttu-id="2680f-131">A következő engedélykéréseket tetején található a fájl hozzáadása után a **használatával** utasítások és előtt a **névtér** nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="2680f-131">Add the following permission requests at the top of the file, after the **using** statements and before the **namespace** declaration.</span></span>

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. <span data-ttu-id="2680f-132">A következő osztálydefiníció hozzáadása a névtérhez.</span><span class="sxs-lookup"><span data-stu-id="2680f-132">Add the following class definition to the namespace.</span></span>

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > <span data-ttu-id="2680f-133">Cserélje le **< PROJECT_NUMBER >** a projekt számát, amelyet korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="2680f-133">Replace **<PROJECT_NUMBER>** with your project number you noted earlier.</span></span>    
   >
   >
10. <span data-ttu-id="2680f-134">Cserélje le az üres **GcmService** osztály a következő kóddal, amely az új szórásos receiver használja:</span><span class="sxs-lookup"><span data-stu-id="2680f-134">Replace the empty **GcmService** class with the following code, which uses the new broadcast receiver:</span></span>

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. <span data-ttu-id="2680f-135">Adja hozzá a következő kódot a **GcmService** osztály.</span><span class="sxs-lookup"><span data-stu-id="2680f-135">Add the following code to the **GcmService** class.</span></span> <span data-ttu-id="2680f-136">A rendszer felülírja a **OnRegistered** eseménykezelő és megvalósít egy **regisztrálása** metódust.</span><span class="sxs-lookup"><span data-stu-id="2680f-136">This overrides the **OnRegistered** event handler and implements a **Register** method.</span></span>

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

    <span data-ttu-id="2680f-137">Vegye figyelembe, hogy ezt a kódot használja a `messageParam` paraméter a sablon regisztrációhoz.</span><span class="sxs-lookup"><span data-stu-id="2680f-137">Note that this code uses the `messageParam` parameter in the template registration.</span></span>
12. <span data-ttu-id="2680f-138">Az alábbi kódot, amely megvalósítja az **OnMessage**:</span><span class="sxs-lookup"><span data-stu-id="2680f-138">Add the following code that implements **OnMessage**:</span></span>

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    <span data-ttu-id="2680f-139">Ez a bejövő értesítések kezeli, és elküldi azokat a notification manager megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="2680f-139">This handles incoming notifications and sends them to the notification manager to be displayed.</span></span>
13. <span data-ttu-id="2680f-140">**GcmServiceBase** is szükséges, hogy implementálja a **OnUnRegistered** és **hibára** kezelő módszerrel, amely a következőképpen teheti:</span><span class="sxs-lookup"><span data-stu-id="2680f-140">**GcmServiceBase** also requires you to implement the **OnUnRegistered** and **OnError** handler methods, which you can do as follows:</span></span>

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

<span data-ttu-id="2680f-141">Most már áll készen áll a teszt leküldéses értesítések Android-eszközön vagy az emulátor futnak az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2680f-141">Now, you are ready test push notifications in the app running on an Android device or the emulator.</span></span>

### <a name="test-push-notifications-in-your-android-app"></a><span data-ttu-id="2680f-142">Teszt leküldéses értesítések Android-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2680f-142">Test push notifications in your Android app</span></span>
<span data-ttu-id="2680f-143">Az első két lépés csak egy emulátorának tesztelést esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="2680f-143">The first two steps are required only when you're testing on an emulator.</span></span>

1. <span data-ttu-id="2680f-144">Győződjön meg arról, üzembe vagy hibakeresés egy virtuális eszközön, amely rendelkezik a Google API-k, célként beállítva, az Android virtuális eszközt kezelőjét alább látható módon vannak.</span><span class="sxs-lookup"><span data-stu-id="2680f-144">Make sure that you are deploying to or debugging on a virtual device that has Google APIs set as the target, as shown below in the Android Virtual Device manager.</span></span>
2. <span data-ttu-id="2680f-145">A Google-fiók kattintva vegyen fel új Android-eszköz **alkalmazások** > **beállítások** > **fiók hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2680f-145">Add a Google account to the Android device by clicking **Apps** > **Settings** > **Add account**.</span></span> <span data-ttu-id="2680f-146">Kövesse az utasításokat egy meglévő Google-fiók hozzáadása az eszközt, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="2680f-146">Then follow the prompts to add an existing Google account to the device, or to create a new one.</span></span>
3. <span data-ttu-id="2680f-147">A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal a **Droid** projektre, kattintson **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="2680f-147">In Visual Studio or Xamarin Studio, right-click the **Droid** project and click **Set as startup project**.</span></span>
4. <span data-ttu-id="2680f-148">Kattintson a **futtatása** a projekt felépítéséhez és az Android-eszköz vagy az emulátor indítsa el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2680f-148">Click **Run** to build the project and start the app on your Android device or emulator.</span></span>
5. <span data-ttu-id="2680f-149">Az alkalmazásban írjon be egy feladatot, és kattintson a plusz (**+**) ikonra.</span><span class="sxs-lookup"><span data-stu-id="2680f-149">In the app, type a task, and then click the plus (**+**) icon.</span></span>
6. <span data-ttu-id="2680f-150">Győződjön meg arról, hogy értesítés érkezik, amikor egy elem hozzá van adva.</span><span class="sxs-lookup"><span data-stu-id="2680f-150">Verify that a notification is received when an item is added.</span></span>

## <a name="configure-and-run-the-ios-project-optional"></a><span data-ttu-id="2680f-151">Konfigurálja és futtassa az iOS-projektre (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="2680f-151">Configure and run the iOS project (optional)</span></span>
<span data-ttu-id="2680f-152">Ez a szakasz az iOS-eszközökhöz készült Xamarin iOS-projektek futtatásával foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="2680f-152">This section is for running the Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="2680f-153">Kihagyhatja ezt a részt, ha nem dolgozik iOS-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="2680f-153">You can skip this section if you are not working with iOS devices.</span></span>

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a><span data-ttu-id="2680f-154">Az értesítési központ konfigurálása az APN Szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="2680f-154">Configure the notification hub for APNS</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

<span data-ttu-id="2680f-155">Az iOS-projekt beállítása a következő konfigurál a Xamarin Studióban vagy a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2680f-155">Next, you will configure the iOS project setting in Xamarin Studio or Visual Studio.</span></span>

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a><span data-ttu-id="2680f-156">Leküldéses értesítések az iOS-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2680f-156">Add push notifications to your iOS app</span></span>
1. <span data-ttu-id="2680f-157">Az a **iOS** projektre, nyissa meg a AppDelegate.cs, és adja hozzá a következő utasítás a kód fájl elejéhez.</span><span class="sxs-lookup"><span data-stu-id="2680f-157">In the **iOS** project, open AppDelegate.cs and add the following statement to the top of the code file.</span></span>

        using Newtonsoft.Json.Linq;
2. <span data-ttu-id="2680f-158">Az a **AppDelegate** osztály, vegyen fel egy felülbírálást a **RegisteredForRemoteNotifications** esemény értesítések regisztrálása:</span><span class="sxs-lookup"><span data-stu-id="2680f-158">In the **AppDelegate** class, add an override for the **RegisteredForRemoteNotifications** event to register for notifications:</span></span>

        public override void RegisteredForRemoteNotifications(UIApplication application,
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }
3. <span data-ttu-id="2680f-159">A **AppDelegate**, a következő felülbírálást is hozzáadhat a **DidReceiveRemoteNotification** eseménykezelő:</span><span class="sxs-lookup"><span data-stu-id="2680f-159">In **AppDelegate**, also add the following override for the **DidReceiveRemoteNotification** event handler:</span></span>

        public override void DidReceiveRemoteNotification(UIApplication application,
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    <span data-ttu-id="2680f-160">Ez a metódus kezeli a bejövő értesítések az alkalmazás futtatása közben.</span><span class="sxs-lookup"><span data-stu-id="2680f-160">This method handles incoming notifications while the app is running.</span></span>
4. <span data-ttu-id="2680f-161">Az a **AppDelegate** osztály, adja hozzá a következő kódot a **FinishedLaunching** módszert:</span><span class="sxs-lookup"><span data-stu-id="2680f-161">In the **AppDelegate** class, add the following code to the **FinishedLaunching** method:</span></span>

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    <span data-ttu-id="2680f-162">Ez lehetővé teszi a távoli értesítések támogatása, és a kérelmek leküldéses regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="2680f-162">This enables support for remote notifications and requests push registration.</span></span>

<span data-ttu-id="2680f-163">Az alkalmazás most frissíteni leküldéses értesítések támogatásához használható.</span><span class="sxs-lookup"><span data-stu-id="2680f-163">Your app is now updated to support push notifications.</span></span>

#### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="2680f-164">Teszt leküldéses értesítéseket az iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2680f-164">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="2680f-165">Kattintson a jobb gombbal az iOS-projektre, és kattintson a **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="2680f-165">Right-click the iOS project, and click **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="2680f-166">Nyomja meg a **futtatása** gomb vagy **F5** a projekt felépítéséhez és az alkalmazás elindításához az iOS-eszközök Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2680f-166">Press the **Run** button or **F5** in Visual Studio to build the project and start the app in an iOS device.</span></span> <span data-ttu-id="2680f-167">Kattintson a **OK** leküldéses értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="2680f-167">Then click **OK** to accept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2680f-168">Az alkalmazásból explicit módon el kell fogadnia a leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="2680f-168">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="2680f-169">A kérelem csak akkor történik meg az első alkalommal futtatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2680f-169">This request only occurs the first time that the app runs.</span></span>
   >
   >
3. <span data-ttu-id="2680f-170">Az alkalmazásban írjon be egy feladatot, és kattintson a plusz (**+**) ikonra.</span><span class="sxs-lookup"><span data-stu-id="2680f-170">In the app, type a task, and then click the plus (**+**) icon.</span></span>
4. <span data-ttu-id="2680f-171">Győződjön meg arról, hogy értesítést kap, és kattintson **OK** az értesítés bezárásának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2680f-171">Verify that a notification is received, and then click **OK** to dismiss the notification.</span></span>

## <a name="configure-and-run-windows-projects-optional"></a><span data-ttu-id="2680f-172">Konfigurálja és futtassa a Windows-projektek (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="2680f-172">Configure and run Windows projects (optional)</span></span>
<span data-ttu-id="2680f-173">Ez a szakasz a a Xamarin.Forms WinApp és a Windows-eszközök WinPhone81 projektek futtatására szolgál.</span><span class="sxs-lookup"><span data-stu-id="2680f-173">This section is for running the Xamarin.Forms WinApp and WinPhone81 projects for Windows devices.</span></span> <span data-ttu-id="2680f-174">Ezeket a lépéseket is támogatja az univerzális Windows Platform (UWP) projektek.</span><span class="sxs-lookup"><span data-stu-id="2680f-174">These steps also support Universal Windows Platform (UWP) projects.</span></span> <span data-ttu-id="2680f-175">Kihagyhatja ezt a részt, ha nem dolgozik Windows-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="2680f-175">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a><span data-ttu-id="2680f-176">Alkalmazás regisztrálása a Windows leküldéses értesítéseket a Windows értesítési szolgáltatása (WNS)</span><span class="sxs-lookup"><span data-stu-id="2680f-176">Register your Windows app for push notifications with Windows Notification Service (WNS)</span></span>
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a><span data-ttu-id="2680f-177">A WNS az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2680f-177">Configure the notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a><span data-ttu-id="2680f-178">Leküldéses értesítések hozzáadása a Windows-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2680f-178">Add push notifications to your Windows app</span></span>
1. <span data-ttu-id="2680f-179">A Visual Studióban nyissa meg a **App.xaml.cs** Windows projektre, és adja hozzá az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2680f-179">In Visual Studio, open **App.xaml.cs** in a Windows project, and add the following statements.</span></span>

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    <span data-ttu-id="2680f-180">Cserélje le `<your_TodoItemManager_portable_class_namespace>` rájuk a névtérrel, a hordozható projekt, amely tartalmazza a `TodoItemManager` osztály.</span><span class="sxs-lookup"><span data-stu-id="2680f-180">Replace `<your_TodoItemManager_portable_class_namespace>` with the namespace of your portable project that contains the `TodoItemManager` class.</span></span>
2. <span data-ttu-id="2680f-181">App.xaml.cs fájlban adja hozzá a következő **InitNotificationsAsync** módszert:</span><span class="sxs-lookup"><span data-stu-id="2680f-181">In App.xaml.cs, add the following **InitNotificationsAsync** method:</span></span>

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS =
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    <span data-ttu-id="2680f-182">Ez a módszer lekérdezi a leküldéses értesítési csatornát, és regisztrál az értesítési központ sablon értesítések fogadása sablon.</span><span class="sxs-lookup"><span data-stu-id="2680f-182">This method gets the push notification channel, and registers a template to receive template notifications from your notification hub.</span></span> <span data-ttu-id="2680f-183">Egy sablon értesítés, amely támogatja a *messageParam* kézbesíti a rendszer az ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="2680f-183">A template notification that supports *messageParam* will be delivered to this client.</span></span>
3. <span data-ttu-id="2680f-184">App.xaml.cs fájlban frissítse a **OnLaunched** esemény kezelő metódusdefiníciót hozzáadásával a `async` módosítóval.</span><span class="sxs-lookup"><span data-stu-id="2680f-184">In App.xaml.cs, update the **OnLaunched** event handler method definition by adding the `async` modifier.</span></span> <span data-ttu-id="2680f-185">Majd adja hozzá a következő kódsort a metódus végén:</span><span class="sxs-lookup"><span data-stu-id="2680f-185">Then add the following line of code at the end of the method:</span></span>

        await InitNotificationsAsync();

    <span data-ttu-id="2680f-186">Ez biztosítja, hogy a leküldéses értesítési regisztrációban létrehozásakor vagy minden alkalommal frissíteni az alkalmazást elindítja.</span><span class="sxs-lookup"><span data-stu-id="2680f-186">This ensures that the push notification registration is created or refreshed every time the app is launched.</span></span> <span data-ttu-id="2680f-187">Fontos ezzel garantálható, hogy a WNS leküldéses csatorna mindig aktív.</span><span class="sxs-lookup"><span data-stu-id="2680f-187">It's important to do this to guarantee that the WNS push channel is always active.</span></span>  
4. <span data-ttu-id="2680f-188">A Visual Studio Solution Explorerben nyissa meg a **Package.appxmanifest** fájlt, és állítsa be **bejelentési képes** való **Igen** alatt **értesítések**.</span><span class="sxs-lookup"><span data-stu-id="2680f-188">In Solution Explorer for Visual Studio, open the **Package.appxmanifest** file, and set **Toast Capable** to **Yes** under **Notifications**.</span></span>
5. <span data-ttu-id="2680f-189">Az alkalmazás elkészítésére, és ellenőrizze, rendelkezik-e hibák.</span><span class="sxs-lookup"><span data-stu-id="2680f-189">Build the app and verify you have no errors.</span></span> <span data-ttu-id="2680f-190">Az ügyfélalkalmazás most regisztrálni kell a sablon értesítések a Mobile Apps háttérből.</span><span class="sxs-lookup"><span data-stu-id="2680f-190">Your client app should now register for the template notifications from the Mobile Apps back end.</span></span> <span data-ttu-id="2680f-191">Ismételje meg minden Windows-projektet a megoldásban ez a szakasz.</span><span class="sxs-lookup"><span data-stu-id="2680f-191">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="2680f-192">Teszt leküldéses értesítések a Windows-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="2680f-192">Test push notifications in your Windows app</span></span>
1. <span data-ttu-id="2680f-193">A Visual Studióban, kattintson a jobb gombbal egy Windows-projektet, majd kattintson **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="2680f-193">In Visual Studio, right-click a Windows project, and click **Set as startup project**.</span></span>
2. <span data-ttu-id="2680f-194">Nyomja le a **Run** (Futtatás) gombot a projekt felépítéséhez és az alkalmazás elindításához.</span><span class="sxs-lookup"><span data-stu-id="2680f-194">Press the **Run** button to build the project and start the app.</span></span>
3. <span data-ttu-id="2680f-195">Az alkalmazásban írjon be egy nevet az új beállíthatnánk, és kattintson a plusz (**+**) ikonra kattintva vegye fel azt.</span><span class="sxs-lookup"><span data-stu-id="2680f-195">In the app, type a name for a new todoitem, and then click the plus (**+**) icon to add it.</span></span>
4. <span data-ttu-id="2680f-196">Győződjön meg arról, hogy értesítést kapott, a cikk felvételekor.</span><span class="sxs-lookup"><span data-stu-id="2680f-196">Verify that a notification is received when the item is added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2680f-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2680f-197">Next steps</span></span>
<span data-ttu-id="2680f-198">További tudnivalók leküldéses értesítések:</span><span class="sxs-lookup"><span data-stu-id="2680f-198">You can learn more about push notifications:</span></span>

* [<span data-ttu-id="2680f-199">Leküldéses értesítési eseményadatokat</span><span class="sxs-lookup"><span data-stu-id="2680f-199">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="2680f-200">Oka különböző miért kerülhetnek vagy értesítések nem végül az eszközökön.</span><span class="sxs-lookup"><span data-stu-id="2680f-200">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="2680f-201">Ez a témakör bemutatja, hogyan elemezheti és mérje fel, az alapvető ok leküldéses értesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2680f-201">This topic shows you how to analyze and figure out the root cause of push notification failures.</span></span>

<span data-ttu-id="2680f-202">Is továbbra is valamelyik további anyagra:</span><span class="sxs-lookup"><span data-stu-id="2680f-202">You can also continue on to one of the following tutorials:</span></span>

* [<span data-ttu-id="2680f-203">Hitelesítés hozzáadása az alkalmazáshoz </span><span class="sxs-lookup"><span data-stu-id="2680f-203">Add authentication to your app </span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="2680f-204">Ismerje meg, hogyan hitelesítheti az alkalmazás felhasználóit egy identitásszolgáltatóval.</span><span class="sxs-lookup"><span data-stu-id="2680f-204">Learn how to authenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="2680f-205">Az offline szinkronizálás engedélyezése az alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="2680f-205">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="2680f-206">Ismerje meg, hogyan adhat offline támogatást alkalmazásához egy Mobile Apps-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="2680f-206">Learn how to add offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="2680f-207">Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="2680f-207">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
