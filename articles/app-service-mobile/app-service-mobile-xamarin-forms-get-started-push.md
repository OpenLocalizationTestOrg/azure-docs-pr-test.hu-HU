---
title: "aaaAdd leküldéses értesítések tooyour Xamarin.Forms-alkalmazás |} Microsoft Docs"
description: "Ismerje meg, hogy miként toouse Azure szolgáltatásokat toosend többplatformos leküldéses értesítések tooyour Xamarin.Forms alkalmazások."
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
ms.openlocfilehash: 9133a0b6dd99c01def525607c20ce5a9c19b9502
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-xamarinforms-app"></a><span data-ttu-id="9d3f5-103">Leküldéses értesítések tooyour Xamarin.Forms-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d3f5-103">Add push notifications tooyour Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="9d3f5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9d3f5-104">Overview</span></span>
<span data-ttu-id="9d3f5-105">Ebben az oktatóanyagban leküldéses értesítések tooall hello projektek hello eredményeként hozzáadása [Xamarin.Forms gyors üzembe helyezési](app-service-mobile-xamarin-forms-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9d3f5-105">In this tutorial, you add push notifications tooall hello projects that resulted from hello [Xamarin.Forms quick start](app-service-mobile-xamarin-forms-get-started.md).</span></span> <span data-ttu-id="9d3f5-106">Ez azt jelenti, hogy egy leküldéses értesítést küld tooall platformfüggetlen ügyfelek minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-106">This means that a push notification is sent tooall cross-platform clients every time a record is inserted.</span></span>

<span data-ttu-id="9d3f5-107">Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-107">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="9d3f5-108">További információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="9d3f5-108">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d3f5-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9d3f5-109">Prerequisites</span></span>
<span data-ttu-id="9d3f5-110">Az iOS, szüksége lesz egy [Apple Fejlesztőprogrambeli tagság](https://developer.apple.com/programs/ios/) és egy fizikai iOS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-110">For iOS, you will need an [Apple Developer Program membership](https://developer.apple.com/programs/ios/) and a physical iOS device.</span></span> <span data-ttu-id="9d3f5-111">Hello [iOS-szimulátorban nem támogatja a leküldéses értesítések](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="9d3f5-111">hello [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span>

## <span data-ttu-id="9d3f5-112"><a name="configure-hub"></a>Egy értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d3f5-112"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a><span data-ttu-id="9d3f5-113">Frissítési hello server projekt toosend leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="9d3f5-113">Update hello server project toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-hello-android-project-optional"></a><span data-ttu-id="9d3f5-114">Konfigurálására és futtatására hello Androidos projekt (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="9d3f5-114">Configure and run hello Android project (optional)</span></span>
<span data-ttu-id="9d3f5-115">Végezze el a szakasz tooenable leküldéses értesítések hello Xamarin.Forms Droid-projekt az Android.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-115">Complete this section tooenable push notifications for hello Xamarin.Forms Droid project for Android.</span></span>

### <a name="enable-firebase-cloud-messaging-fcm"></a><span data-ttu-id="9d3f5-116">Firebase Cloud Messaging (FCM) engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9d3f5-116">Enable Firebase Cloud Messaging (FCM)</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-hello-mobile-apps-back-end-toosend-push-requests-by-using-fcm"></a><span data-ttu-id="9d3f5-117">Konfigurálhatja a hello Mobile Apps háttér toosend leküldéses kérések FCM használatával</span><span class="sxs-lookup"><span data-stu-id="9d3f5-117">Configure hello Mobile Apps back end toosend push requests by using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-toohello-android-project"></a><span data-ttu-id="9d3f5-118">Leküldéses értesítések toohello Android-projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d3f5-118">Add push notifications toohello Android project</span></span>
<span data-ttu-id="9d3f5-119">Hello háttérből FCM konfigurálva, az összetevők és kódok toohello ügyfél tooregister FCM a is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-119">With hello back end configured with FCM, you can add components and codes toohello client tooregister with FCM.</span></span> <span data-ttu-id="9d3f5-120">A leküldéses értesítések az Azure Notification Hubs hello vissza a Mobile Apps végén, és értesítéseket kaphat keresztül is rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-120">You can also register for push notifications with Azure Notification Hubs through hello Mobile Apps back end, and receive notifications.</span></span>

1. <span data-ttu-id="9d3f5-121">A hello **Droid** projektre, kattintson a jobb gombbal a hello **összetevők** mappára, majd kattintson **több összetevők beolvasása...** . Majd keresse meg a hello **Google Cloud Messaging Client** összetevő, és adja hozzá toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-121">In hello **Droid** project, right-click hello **Components** folder, and click **Get More Components...**. Then search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span> <span data-ttu-id="9d3f5-122">Ez az összetevő a Xamarin Android-projekt leküldéses értesítések támogat.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-122">This component supports push notifications for a Xamarin Android project.</span></span>
2. <span data-ttu-id="9d3f5-123">Nyissa meg a hello MainActivity.cs projektfájlt, és adja hozzá a következő utasítás hello fájl hello tetején hello:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-123">Open hello MainActivity.cs project file, and add hello following statement at hello top of hello file:</span></span>

        using Gcm.Client;
3. <span data-ttu-id="9d3f5-124">Adja hozzá a következő kód toohello hello **OnCreate** után hello kiszolgálómetódus-hívás túl**LoadApplication**:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-124">Add hello following code toohello **OnCreate** method after hello call too**LoadApplication**:</span></span>

        try
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating hello client. Verify hello URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. <span data-ttu-id="9d3f5-125">Adjon hozzá egy új **CreateAndShowDialog** segítő módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-125">Add a new **CreateAndShowDialog** helper method, as follows:</span></span>

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. <span data-ttu-id="9d3f5-126">Adja hozzá a következő kód toohello hello **MainActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-126">Add hello following code toohello **MainActivity** class:</span></span>

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return hello current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    <span data-ttu-id="9d3f5-127">Ez mutatja a hello aktuális **MainActivity** példányt, így a hello fő felhasználói felület szálán végezhetünk.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-127">This exposes hello current **MainActivity** instance, so we can execute on hello main UI thread.</span></span>
6. <span data-ttu-id="9d3f5-128">Hello inicializálása `instance` változó hello hello elején **OnCreate** módszert az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-128">Initialize hello `instance` variable at hello beginning of hello **OnCreate** method, as follows.</span></span>

        // Set hello current instance of MainActivity.
        instance = this;
7. <span data-ttu-id="9d3f5-129">Adja hozzá egy új osztályt fájl toohello **Droid** nevű projekt `GcmService.cs`, és győződjön meg arról, hogy hello következő **használatával** utasítások hello fájl hello tetején találhatók:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-129">Add a new class file toohello **Droid** project named `GcmService.cs`, and make sure hello following **using** statements are present at hello top of hello file:</span></span>

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
8. <span data-ttu-id="9d3f5-130">Adja hozzá a következő engedélykéréseket hello fájl hello tetején után hello hello **használatával** utasítások és előtt hello **névtér** nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-130">Add hello following permission requests at hello top of hello file, after hello **using** statements and before hello **namespace** declaration.</span></span>

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. <span data-ttu-id="9d3f5-131">Adja hozzá a következő osztály definition toohello névtér hello.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-131">Add hello following class definition toohello namespace.</span></span>

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > <span data-ttu-id="9d3f5-132">Cserélje le **< PROJECT_NUMBER >** a projekt számát, amelyet korábban feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-132">Replace **<PROJECT_NUMBER>** with your project number you noted earlier.</span></span>    
   >
   >
10. <span data-ttu-id="9d3f5-133">Cserélje le az üres hello **GcmService** hello kódot, amely hello új szórásos receiver használja a következő osztályra:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-133">Replace hello empty **GcmService** class with hello following code, which uses hello new broadcast receiver:</span></span>

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. <span data-ttu-id="9d3f5-134">Adja hozzá a következő kód toohello hello **GcmService** osztály.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-134">Add hello following code toohello **GcmService** class.</span></span> <span data-ttu-id="9d3f5-135">A rendszer felülírja a hello **OnRegistered** eseménykezelő és megvalósít egy **regisztrálása** metódust.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-135">This overrides hello **OnRegistered** event handler and implements a **Register** method.</span></span>

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

    <span data-ttu-id="9d3f5-136">Vegye figyelembe, hogy ezt a kódot használja hello `messageParam` hello sablon regisztrációs paramétere.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-136">Note that this code uses hello `messageParam` parameter in hello template registration.</span></span>
12. <span data-ttu-id="9d3f5-137">Adja hozzá a következő kódot, amely megvalósítja az hello **OnMessage**:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-137">Add hello following code that implements **OnMessage**:</span></span>

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store hello message
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

            //Create an intent tooshow ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create hello notification
            //we use hello pending intent, passing our ui intent over which will get called
            //when hello notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set hello notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove hello notification once hello user touches it
                    .SetAutoCancel(true).Build();

            //Show hello notification
            notificationManager.Notify(1, notification);
        }

    <span data-ttu-id="9d3f5-138">Ez a bejövő értesítések kezeli, és elküldi azokat toohello notification manager toobe jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-138">This handles incoming notifications and sends them toohello notification manager toobe displayed.</span></span>
13. <span data-ttu-id="9d3f5-139">**GcmServiceBase** is meg tooimplement hello **OnUnRegistered** és **hibára** kezelő módszerrel, amely a következőképpen teheti:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-139">**GcmServiceBase** also requires you tooimplement hello **OnUnRegistered** and **OnError** handler methods, which you can do as follows:</span></span>

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

<span data-ttu-id="9d3f5-140">Most már készen áll a teszt leküldéses értesítések Android-eszközön futó hello alkalmazást a rendszer, vagy emulátor hello.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-140">Now, you are ready test push notifications in hello app running on an Android device or hello emulator.</span></span>

### <a name="test-push-notifications-in-your-android-app"></a><span data-ttu-id="9d3f5-141">Teszt leküldéses értesítések Android-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="9d3f5-141">Test push notifications in your Android app</span></span>
<span data-ttu-id="9d3f5-142">hello két lépést csak egy emulátorának tesztelést esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-142">hello first two steps are required only when you're testing on an emulator.</span></span>

1. <span data-ttu-id="9d3f5-143">Győződjön meg arról, hogy telepít egy virtuális eszközön, amely rendelkezik a Google API-k hello célként beállítva hello Android virtuális eszközt kezelő az alábbiak szerint hibakeresés tooor.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-143">Make sure that you are deploying tooor debugging on a virtual device that has Google APIs set as hello target, as shown below in hello Android Virtual Device manager.</span></span>
2. <span data-ttu-id="9d3f5-144">Google fiók toohello Android-eszköz hozzáadásához kattintva **alkalmazások** > **beállítások** > **fiók hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-144">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**.</span></span> <span data-ttu-id="9d3f5-145">Kövesse hello kér tooadd egy meglévő Google-fiók toohello eszközt, vagy toocreate egy újat.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-145">Then follow hello prompts tooadd an existing Google account toohello device, or toocreate a new one.</span></span>
3. <span data-ttu-id="9d3f5-146">A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal hello **Droid** projektre, kattintson **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-146">In Visual Studio or Xamarin Studio, right-click hello **Droid** project and click **Set as startup project**.</span></span>
4. <span data-ttu-id="9d3f5-147">Kattintson a **futtatása** toobuild hello projektet, és indítsa el a hello alkalmazást az Android-eszköz vagy az emulátor.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-147">Click **Run** toobuild hello project and start hello app on your Android device or emulator.</span></span>
5. <span data-ttu-id="9d3f5-148">Hello alkalmazásban írjon be egy feladatot, és kattintson a hello plusz (**+**) ikonra.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-148">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
6. <span data-ttu-id="9d3f5-149">Győződjön meg arról, hogy értesítés érkezik, amikor egy elem hozzá van adva.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-149">Verify that a notification is received when an item is added.</span></span>

## <a name="configure-and-run-hello-ios-project-optional"></a><span data-ttu-id="9d3f5-150">Konfigurálására és futtatására hello iOS-projektre (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="9d3f5-150">Configure and run hello iOS project (optional)</span></span>
<span data-ttu-id="9d3f5-151">Ez a szakasz hello Xamarin iOS-projektet az iOS-eszközök futtatására szolgál.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-151">This section is for running hello Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="9d3f5-152">Kihagyhatja ezt a részt, ha nem dolgozik iOS-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-152">You can skip this section if you are not working with iOS devices.</span></span>

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-hello-notification-hub-for-apns"></a><span data-ttu-id="9d3f5-153">Az APNS hello értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d3f5-153">Configure hello notification hub for APNS</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

<span data-ttu-id="9d3f5-154">A következő hello iOS-projekt beállítása konfigurál a Xamarin Studióban vagy a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-154">Next, you will configure hello iOS project setting in Xamarin Studio or Visual Studio.</span></span>

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-tooyour-ios-app"></a><span data-ttu-id="9d3f5-155">Leküldéses értesítések tooyour iOS-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d3f5-155">Add push notifications tooyour iOS app</span></span>
1. <span data-ttu-id="9d3f5-156">A hello **iOS** projektre, nyissa meg a AppDelegate.cs, és adja hozzá a következő utasítás toohello felső hello kódfájl hello.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-156">In hello **iOS** project, open AppDelegate.cs and add hello following statement toohello top of hello code file.</span></span>

        using Newtonsoft.Json.Linq;
2. <span data-ttu-id="9d3f5-157">A hello **AppDelegate** osztály, vegyen fel egy felülbírálást a hello **RegisteredForRemoteNotifications** esemény tooregister értesítéseket:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-157">In hello **AppDelegate** class, add an override for hello **RegisteredForRemoteNotifications** event tooregister for notifications:</span></span>

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
3. <span data-ttu-id="9d3f5-158">A **AppDelegate**, is hozzáadhat a következő hello felülbírálást hello **DidReceiveRemoteNotification** eseménykezelő:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-158">In **AppDelegate**, also add hello following override for hello **DidReceiveRemoteNotification** event handler:</span></span>

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

    <span data-ttu-id="9d3f5-159">Ez a metódus kezeli a bejövő értesítések hello alkalmazás futtatása közben.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-159">This method handles incoming notifications while hello app is running.</span></span>
4. <span data-ttu-id="9d3f5-160">A hello **AppDelegate** osztály, adja hozzá a következő kód toohello hello **FinishedLaunching** módszert:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-160">In hello **AppDelegate** class, add hello following code toohello **FinishedLaunching** method:</span></span>

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    <span data-ttu-id="9d3f5-161">Ez lehetővé teszi a távoli értesítések támogatása, és a kérelmek leküldéses regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-161">This enables support for remote notifications and requests push registration.</span></span>

<span data-ttu-id="9d3f5-162">Az alkalmazás már frissített toosupport leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-162">Your app is now updated toosupport push notifications.</span></span>

#### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="9d3f5-163">Teszt leküldéses értesítéseket az iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="9d3f5-163">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="9d3f5-164">Kattintson a jobb gombbal a hello iOS-projektre, és kattintson a **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-164">Right-click hello iOS project, and click **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="9d3f5-165">Nyomja le az hello **futtatása** gomb vagy **F5** a Visual Studio toobuild hello projektet, és indítsa el hello alkalmazást iOS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-165">Press hello **Run** button or **F5** in Visual Studio toobuild hello project and start hello app in an iOS device.</span></span> <span data-ttu-id="9d3f5-166">Kattintson a **OK** tooaccept leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-166">Then click **OK** tooaccept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9d3f5-167">Az alkalmazásból explicit módon el kell fogadnia a leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-167">You must explicitly accept push notifications from your app.</span></span> <span data-ttu-id="9d3f5-168">A kérelem csak akkor történik meg hello először, hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-168">This request only occurs hello first time that hello app runs.</span></span>
   >
   >
3. <span data-ttu-id="9d3f5-169">Hello alkalmazásban írjon be egy feladatot, és kattintson a hello plusz (**+**) ikonra.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-169">In hello app, type a task, and then click hello plus (**+**) icon.</span></span>
4. <span data-ttu-id="9d3f5-170">Győződjön meg arról, hogy értesítést kap, és kattintson **OK** toodismiss hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-170">Verify that a notification is received, and then click **OK** toodismiss hello notification.</span></span>

## <a name="configure-and-run-windows-projects-optional"></a><span data-ttu-id="9d3f5-171">Konfigurálja és futtassa a Windows-projektek (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="9d3f5-171">Configure and run Windows projects (optional)</span></span>
<span data-ttu-id="9d3f5-172">Ez a szakasz a hello Windows-eszközök Xamarin.Forms WinApp és WinPhone81 projektek futtatására szolgál.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-172">This section is for running hello Xamarin.Forms WinApp and WinPhone81 projects for Windows devices.</span></span> <span data-ttu-id="9d3f5-173">Ezeket a lépéseket is támogatja az univerzális Windows Platform (UWP) projektek.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-173">These steps also support Universal Windows Platform (UWP) projects.</span></span> <span data-ttu-id="9d3f5-174">Kihagyhatja ezt a részt, ha nem dolgozik Windows-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-174">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a><span data-ttu-id="9d3f5-175">Alkalmazás regisztrálása a Windows leküldéses értesítéseket a Windows értesítési szolgáltatása (WNS)</span><span class="sxs-lookup"><span data-stu-id="9d3f5-175">Register your Windows app for push notifications with Windows Notification Service (WNS)</span></span>
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-hello-notification-hub-for-wns"></a><span data-ttu-id="9d3f5-176">A WNS hello értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d3f5-176">Configure hello notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-tooyour-windows-app"></a><span data-ttu-id="9d3f5-177">Leküldéses értesítések tooyour Windows-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d3f5-177">Add push notifications tooyour Windows app</span></span>
1. <span data-ttu-id="9d3f5-178">A Visual Studióban nyissa meg a **App.xaml.cs** Windows projektre, majd adja meg a következő utasítások hello.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-178">In Visual Studio, open **App.xaml.cs** in a Windows project, and add hello following statements.</span></span>

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    <span data-ttu-id="9d3f5-179">Cserélje le `<your_TodoItemManager_portable_class_namespace>` hello névtér a hordozható projekt hello tartalmazó `TodoItemManager` osztály.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-179">Replace `<your_TodoItemManager_portable_class_namespace>` with hello namespace of your portable project that contains hello `TodoItemManager` class.</span></span>
2. <span data-ttu-id="9d3f5-180">App.xaml.cs fájlban adja hozzá a hello következő **InitNotificationsAsync** módszert:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-180">In App.xaml.cs, add hello following **InitNotificationsAsync** method:</span></span>

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

    <span data-ttu-id="9d3f5-181">Ez a módszer lekérdezi hello leküldéses értesítési csatornát, és regisztrálja egy sablon tooreceive sablon értesítést kapnak az értesítési központ.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-181">This method gets hello push notification channel, and registers a template tooreceive template notifications from your notification hub.</span></span> <span data-ttu-id="9d3f5-182">Egy sablon értesítés, amely támogatja a *messageParam* toothis ügyfél érkeznek.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-182">A template notification that supports *messageParam* will be delivered toothis client.</span></span>
3. <span data-ttu-id="9d3f5-183">App.xaml.cs fájlban frissítse a hello **OnLaunched** esemény kezelő metódusdefiníciót hello hozzáadásával `async` módosítóval.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-183">In App.xaml.cs, update hello **OnLaunched** event handler method definition by adding hello `async` modifier.</span></span> <span data-ttu-id="9d3f5-184">Majd adja hozzá a következő kódsort hello metódus hello végén hello:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-184">Then add hello following line of code at hello end of hello method:</span></span>

        await InitNotificationsAsync();

    <span data-ttu-id="9d3f5-185">Ez biztosítja, hogy a hello leküldéses értesítési regisztrációban jön létre vagy minden alkalommal frissíteni hello alkalmazás elindul.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-185">This ensures that hello push notification registration is created or refreshed every time hello app is launched.</span></span> <span data-ttu-id="9d3f5-186">Fontos toodo a tooguarantee, amely a WNS leküldéses csatorna hello mindig aktív.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-186">It's important toodo this tooguarantee that hello WNS push channel is always active.</span></span>  
4. <span data-ttu-id="9d3f5-187">A Visual Studio Solution Explorerben nyissa meg a hello **Package.appxmanifest** fájlt, és állítsa be **bejelentési képes** túl**Igen** alatt **értesítések**.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-187">In Solution Explorer for Visual Studio, open hello **Package.appxmanifest** file, and set **Toast Capable** too**Yes** under **Notifications**.</span></span>
5. <span data-ttu-id="9d3f5-188">Hello alkalmazás elkészítésére, és ellenőrizze, rendelkezik-e hibák.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-188">Build hello app and verify you have no errors.</span></span> <span data-ttu-id="9d3f5-189">Az ügyfélalkalmazás most értesítéseihez hello sablon hello vissza a Mobile Apps végén kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-189">Your client app should now register for hello template notifications from hello Mobile Apps back end.</span></span> <span data-ttu-id="9d3f5-190">Ismételje meg minden Windows-projektet a megoldásban ez a szakasz.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-190">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="9d3f5-191">Teszt leküldéses értesítések a Windows-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="9d3f5-191">Test push notifications in your Windows app</span></span>
1. <span data-ttu-id="9d3f5-192">A Visual Studióban, kattintson a jobb gombbal egy Windows-projektet, majd kattintson **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-192">In Visual Studio, right-click a Windows project, and click **Set as startup project**.</span></span>
2. <span data-ttu-id="9d3f5-193">Nyomja le az hello **futtatása** toobuild hello projekt gombra, majd indítsa el hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-193">Press hello **Run** button toobuild hello project and start hello app.</span></span>
3. <span data-ttu-id="9d3f5-194">Hello alkalmazást, és írja be egy új todoitem nevét, és kattintson a hello plusz (**+**) ikonra tooadd azt.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-194">In hello app, type a name for a new todoitem, and then click hello plus (**+**) icon tooadd it.</span></span>
4. <span data-ttu-id="9d3f5-195">Győződjön meg arról, hogy értesítést kapott, hello cikk felvételekor.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-195">Verify that a notification is received when hello item is added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d3f5-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d3f5-196">Next steps</span></span>
<span data-ttu-id="9d3f5-197">További tudnivalók leküldéses értesítések:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-197">You can learn more about push notifications:</span></span>

* [<span data-ttu-id="9d3f5-198">Leküldéses értesítési eseményadatokat</span><span class="sxs-lookup"><span data-stu-id="9d3f5-198">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="9d3f5-199">Oka különböző miért kerülhetnek vagy értesítések nem végül az eszközökön.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-199">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="9d3f5-200">Ez a témakör bemutatja, hogyan tooanalyze és vizsgálhatja meg hello legfelső szintű okozhatják a leküldéses értesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-200">This topic shows you how tooanalyze and figure out hello root cause of push notification failures.</span></span>

<span data-ttu-id="9d3f5-201">Akkor is folytatódhat a tooone az alábbi oktatóanyagok hello:</span><span class="sxs-lookup"><span data-stu-id="9d3f5-201">You can also continue on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="9d3f5-202">Hitelesítési tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d3f5-202">Add authentication tooyour app </span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="9d3f5-203">Ismerje meg, hogy az alkalmazás egy identitásszolgáltatóval tooauthenticate felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-203">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="9d3f5-204">Az offline szinkronizálás engedélyezése az alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="9d3f5-204">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="9d3f5-205">Megtudhatja, hogyan háttér tooadd offline támogatást az alkalmazásához egy Mobile Apps használatával.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-205">Learn how tooadd offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="9d3f5-206">Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="9d3f5-206">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
