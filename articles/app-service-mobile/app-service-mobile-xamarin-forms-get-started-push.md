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
# <a name="add-push-notifications-tooyour-xamarinforms-app"></a>Leküldéses értesítések tooyour Xamarin.Forms-alkalmazás hozzáadása
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Áttekintés
Ebben az oktatóanyagban leküldéses értesítések tooall hello projektek hello eredményeként hozzáadása [Xamarin.Forms gyors üzembe helyezési](app-service-mobile-xamarin-forms-get-started.md). Ez azt jelenti, hogy egy leküldéses értesítést küld tooall platformfüggetlen ügyfelek minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.

Ha nem használja a hello letöltése – első lépések, meg fog kell hello leküldéses értesítési kiterjesztés csomagjában. További információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Előfeltételek
Az iOS, szüksége lesz egy [Apple Fejlesztőprogrambeli tagság](https://developer.apple.com/programs/ios/) és egy fizikai iOS-eszközön. Hello [iOS-szimulátorban nem támogatja a leküldéses értesítések](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Egy értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-hello-server-project-toosend-push-notifications"></a>Frissítési hello server projekt toosend leküldéses értesítések
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-hello-android-project-optional"></a>Konfigurálására és futtatására hello Androidos projekt (nem kötelező)
Végezze el a szakasz tooenable leküldéses értesítések hello Xamarin.Forms Droid-projekt az Android.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Firebase Cloud Messaging (FCM) engedélyezése
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-hello-mobile-apps-back-end-toosend-push-requests-by-using-fcm"></a>Konfigurálhatja a hello Mobile Apps háttér toosend leküldéses kérések FCM használatával
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-toohello-android-project"></a>Leküldéses értesítések toohello Android-projekt hozzáadása
Hello háttérből FCM konfigurálva, az összetevők és kódok toohello ügyfél tooregister FCM a is hozzáadhat. A leküldéses értesítések az Azure Notification Hubs hello vissza a Mobile Apps végén, és értesítéseket kaphat keresztül is rögzítheti.

1. A hello **Droid** projektre, kattintson a jobb gombbal a hello **összetevők** mappára, majd kattintson **több összetevők beolvasása...** . Majd keresse meg a hello **Google Cloud Messaging Client** összetevő, és adja hozzá toohello projekt. Ez az összetevő a Xamarin Android-projekt leküldéses értesítések támogat.
2. Nyissa meg a hello MainActivity.cs projektfájlt, és adja hozzá a következő utasítás hello fájl hello tetején hello:

        using Gcm.Client;
3. Adja hozzá a következő kód toohello hello **OnCreate** után hello kiszolgálómetódus-hívás túl**LoadApplication**:

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
4. Adjon hozzá egy új **CreateAndShowDialog** segítő módszert az alábbiak szerint:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. Adja hozzá a következő kód toohello hello **MainActivity** osztály:

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

    Ez mutatja a hello aktuális **MainActivity** példányt, így a hello fő felhasználói felület szálán végezhetünk.
6. Hello inicializálása `instance` változó hello hello elején **OnCreate** módszert az alábbiak szerint.

        // Set hello current instance of MainActivity.
        instance = this;
7. Adja hozzá egy új osztályt fájl toohello **Droid** nevű projekt `GcmService.cs`, és győződjön meg arról, hogy hello következő **használatával** utasítások hello fájl hello tetején találhatók:

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
8. Adja hozzá a következő engedélykéréseket hello fájl hello tetején után hello hello **használatával** utasítások és előtt hello **névtér** nyilatkozatot.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. Adja hozzá a következő osztály definition toohello névtér hello.

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > Cserélje le **< PROJECT_NUMBER >** a projekt számát, amelyet korábban feljegyzett.    
   >
   >
10. Cserélje le az üres hello **GcmService** hello kódot, amely hello új szórásos receiver használja a következő osztályra:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. Adja hozzá a következő kód toohello hello **GcmService** osztály. A rendszer felülírja a hello **OnRegistered** eseménykezelő és megvalósít egy **regisztrálása** metódust.

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

    Vegye figyelembe, hogy ezt a kódot használja hello `messageParam` hello sablon regisztrációs paramétere.
12. Adja hozzá a következő kódot, amely megvalósítja az hello **OnMessage**:

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

    Ez a bejövő értesítések kezeli, és elküldi azokat toohello notification manager toobe jelenik meg.
13. **GcmServiceBase** is meg tooimplement hello **OnUnRegistered** és **hibára** kezelő módszerrel, amely a következőképpen teheti:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Most már készen áll a teszt leküldéses értesítések Android-eszközön futó hello alkalmazást a rendszer, vagy emulátor hello.

### <a name="test-push-notifications-in-your-android-app"></a>Teszt leküldéses értesítések Android-alkalmazás
hello két lépést csak egy emulátorának tesztelést esetén szükséges.

1. Győződjön meg arról, hogy telepít egy virtuális eszközön, amely rendelkezik a Google API-k hello célként beállítva hello Android virtuális eszközt kezelő az alábbiak szerint hibakeresés tooor.
2. Google fiók toohello Android-eszköz hozzáadásához kattintva **alkalmazások** > **beállítások** > **fiók hozzáadása**. Kövesse hello kér tooadd egy meglévő Google-fiók toohello eszközt, vagy toocreate egy újat.
3. A Visual Studio és Xamarin Studióban, kattintson a jobb gombbal hello **Droid** projektre, kattintson **beállítás kezdőprojektként**.
4. Kattintson a **futtatása** toobuild hello projektet, és indítsa el a hello alkalmazást az Android-eszköz vagy az emulátor.
5. Hello alkalmazásban írjon be egy feladatot, és kattintson a hello plusz (**+**) ikonra.
6. Győződjön meg arról, hogy értesítés érkezik, amikor egy elem hozzá van adva.

## <a name="configure-and-run-hello-ios-project-optional"></a>Konfigurálására és futtatására hello iOS-projektre (nem kötelező)
Ez a szakasz hello Xamarin iOS-projektet az iOS-eszközök futtatására szolgál. Kihagyhatja ezt a részt, ha nem dolgozik iOS-eszközökkel.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-hello-notification-hub-for-apns"></a>Az APNS hello értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

A következő hello iOS-projekt beállítása konfigurál a Xamarin Studióban vagy a Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-tooyour-ios-app"></a>Leküldéses értesítések tooyour iOS-alkalmazás hozzáadása
1. A hello **iOS** projektre, nyissa meg a AppDelegate.cs, és adja hozzá a következő utasítás toohello felső hello kódfájl hello.

        using Newtonsoft.Json.Linq;
2. A hello **AppDelegate** osztály, vegyen fel egy felülbírálást a hello **RegisteredForRemoteNotifications** esemény tooregister értesítéseket:

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
3. A **AppDelegate**, is hozzáadhat a következő hello felülbírálást hello **DidReceiveRemoteNotification** eseménykezelő:

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

    Ez a metódus kezeli a bejövő értesítések hello alkalmazás futtatása közben.
4. A hello **AppDelegate** osztály, adja hozzá a következő kód toohello hello **FinishedLaunching** módszert:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Ez lehetővé teszi a távoli értesítések támogatása, és a kérelmek leküldéses regisztrálása.

Az alkalmazás már frissített toosupport leküldéses értesítéseket.

#### <a name="test-push-notifications-in-your-ios-app"></a>Teszt leküldéses értesítéseket az iOS-alkalmazás
1. Kattintson a jobb gombbal a hello iOS-projektre, és kattintson a **beállítás kezdőprojektként**.
2. Nyomja le az hello **futtatása** gomb vagy **F5** a Visual Studio toobuild hello projektet, és indítsa el hello alkalmazást iOS-eszközön. Kattintson a **OK** tooaccept leküldéses értesítéseket.

   > [!NOTE]
   > Az alkalmazásból explicit módon el kell fogadnia a leküldéses értesítések. A kérelem csak akkor történik meg hello először, hello alkalmazást futtat.
   >
   >
3. Hello alkalmazásban írjon be egy feladatot, és kattintson a hello plusz (**+**) ikonra.
4. Győződjön meg arról, hogy értesítést kap, és kattintson **OK** toodismiss hello értesítést.

## <a name="configure-and-run-windows-projects-optional"></a>Konfigurálja és futtassa a Windows-projektek (nem kötelező)
Ez a szakasz a hello Windows-eszközök Xamarin.Forms WinApp és WinPhone81 projektek futtatására szolgál. Ezeket a lépéseket is támogatja az univerzális Windows Platform (UWP) projektek. Kihagyhatja ezt a részt, ha nem dolgozik Windows-eszközökkel.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Alkalmazás regisztrálása a Windows leküldéses értesítéseket a Windows értesítési szolgáltatása (WNS)
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-hello-notification-hub-for-wns"></a>A WNS hello értesítési központ konfigurálása
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-tooyour-windows-app"></a>Leküldéses értesítések tooyour Windows-alkalmazás hozzáadása
1. A Visual Studióban nyissa meg a **App.xaml.cs** Windows projektre, majd adja meg a következő utasítások hello.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Cserélje le `<your_TodoItemManager_portable_class_namespace>` hello névtér a hordozható projekt hello tartalmazó `TodoItemManager` osztály.
2. App.xaml.cs fájlban adja hozzá a hello következő **InitNotificationsAsync** módszert:

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

    Ez a módszer lekérdezi hello leküldéses értesítési csatornát, és regisztrálja egy sablon tooreceive sablon értesítést kapnak az értesítési központ. Egy sablon értesítés, amely támogatja a *messageParam* toothis ügyfél érkeznek.
3. App.xaml.cs fájlban frissítse a hello **OnLaunched** esemény kezelő metódusdefiníciót hello hozzáadásával `async` módosítóval. Majd adja hozzá a következő kódsort hello metódus hello végén hello:

        await InitNotificationsAsync();

    Ez biztosítja, hogy a hello leküldéses értesítési regisztrációban jön létre vagy minden alkalommal frissíteni hello alkalmazás elindul. Fontos toodo a tooguarantee, amely a WNS leküldéses csatorna hello mindig aktív.  
4. A Visual Studio Solution Explorerben nyissa meg a hello **Package.appxmanifest** fájlt, és állítsa be **bejelentési képes** túl**Igen** alatt **értesítések**.
5. Hello alkalmazás elkészítésére, és ellenőrizze, rendelkezik-e hibák. Az ügyfélalkalmazás most értesítéseihez hello sablon hello vissza a Mobile Apps végén kell regisztrálni. Ismételje meg minden Windows-projektet a megoldásban ez a szakasz.

#### <a name="test-push-notifications-in-your-windows-app"></a>Teszt leküldéses értesítések a Windows-alkalmazásokban
1. A Visual Studióban, kattintson a jobb gombbal egy Windows-projektet, majd kattintson **beállítás kezdőprojektként**.
2. Nyomja le az hello **futtatása** toobuild hello projekt gombra, majd indítsa el hello alkalmazást.
3. Hello alkalmazást, és írja be egy új todoitem nevét, és kattintson a hello plusz (**+**) ikonra tooadd azt.
4. Győződjön meg arról, hogy értesítést kapott, hello cikk felvételekor.

## <a name="next-steps"></a>Következő lépések
További tudnivalók leküldéses értesítések:

* [Leküldéses értesítési eseményadatokat](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Oka különböző miért kerülhetnek vagy értesítések nem végül az eszközökön. Ez a témakör bemutatja, hogyan tooanalyze és vizsgálhatja meg hello legfelső szintű okozhatják a leküldéses értesítés sikertelen.

Akkor is folytatódhat a tooone az alábbi oktatóanyagok hello:

* [Hitelesítési tooyour alkalmazás hozzáadása](app-service-mobile-xamarin-forms-get-started-users.md)  
  Ismerje meg, hogy az alkalmazás egy identitásszolgáltatóval tooauthenticate felhasználóit.
* [Az offline szinkronizálás engedélyezése az alkalmazás számára](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Megtudhatja, hogyan háttér tooadd offline támogatást az alkalmazásához egy Mobile Apps használatával. Kapcsolat nélküli szinkronizálás, a felhasználók használhatják a mobilalkalmazás&mdash;megtekintését, hozzáadását és módosítását adatok&mdash;akkor is, ha nincs hálózati kapcsolat.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
