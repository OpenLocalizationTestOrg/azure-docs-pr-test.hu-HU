---
title: "aaaGet Xamarin.Android-alkalmazásokkal való használatába Notification hubs használatával |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja toouse Azure Notification Hubs toosend a leküldéses értesítések tooa Xamarin Android-alkalmazásokba."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c5c7ead9a9381ab9fb6bbe86e930a8a9254813d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Ismerkedés a Notification Hubs Xamarin Android-alkalmazásokkal való használatával
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Xamarin.Android-alkalmazásba.
Létre fog hozni egy üres Xamarin.Android-alkalmazást, amely leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával. Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató. hello befejezett kód is elérhető hello [NotificationHubs-alkalmazásban] [ GitHub] minta.

Ez az oktatóanyag bemutatja, hogyan hello egyszerű küldési forgatókönyvet a Notification Hubs használatával.

## <a name="before-you-begin"></a>Előkészületek
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

az oktatóanyag befejezése hello kódja a Githubon található [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag hello következő szükséges:

* Windows rendszeren Visual Studio with Xamarin, vagy Mac OS X rendszeren Xamarin Studio. A teljes telepítési útmutatás itt található: [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).
* Aktív Google-fiók
* [Azure Messaging összetevő]
* [Google Cloud Messaging Client összetevő]

Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Xamarin.Android-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.

> [!IMPORTANT]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-google-cloud-messaging"></a>A Google Cloud Messaging engedélyezése
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Az értesítési központ konfigurálása
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p>Kattintson a hello <b>konfigurálása</b> hello felső lapra, adja meg a hello <b>API-kulcs</b> hello előző szakaszban beszerzett értékét, és kattintson a <b>mentése</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)

Az értesítési központ most konfigurált toowork GCM-mel, és az alkalmazás tooreceive értesítések és toosend leküldéses értesítések regisztrálása hello kapcsolati karakterláncok tooboth van.

## <a name="connect-your-app-toohello-notification-hub"></a>Csatlakozás az alkalmazás toohello értesítési központ
### <a name="create-a-new-project"></a>Új projekt létrehozása
1. A Xamarin Studióban kattintson a **New Solution** (Új megoldás), az **Android App** (Android-alkalmazás), majd a **Next** (Tovább) elemre.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Adja meg az **App name** (Alkalmazás neve) és az **Identifier** (Azonosító) értékét. Kattintson a hello **Target Plaforms** toosupport szeretné, majd kattintson **következő** és **létrehozása**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    Ezzel létrehoz egy új Android-projektet.

1. Hello projekt tulajdonságainak megnyitásához kattintson a jobb gombbal az új projektre a hello megoldás nézet, és válasszon **beállítások**. Jelölje be hello **Android-alkalmazás** hello elemére **Build** szakasz.
   
    Győződjön meg arról, hogy hello első betűjének a **csomagnév** értéke kisbetűvel kezdődik.
   
   > [!IMPORTANT]
   > hello hello csomagnév első betűjének kisbetűnek kell lennie. Különben az alkalmazásjegyzékkel kapcsolatos hibák lépnek fel a **BroadcastReceiver** és az **IntentFilter** leküldéses értesítésekre való alábbi regisztrálása során.
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. Szükség esetén állítsa hello **az Android minimálisan** tooanother API-szintet.
3. Szükség esetén állítsa hello **célja az Android** toohello, amelyet az tootarget (kell API-szintet 8-as vagy magasabb) egy másik API-verzió.

Kattintson a **OK** és Bezárás hello projekt beállításai párbeszédpanelen.

### <a name="add-hello-required-components-tooyour-project"></a>Hello szükséges összetevők tooyour projekt hozzáadása
Google Cloud Messaging Client hello Xamarin Component Store áruházban elérhető hello egyszerűbben hello támogathatja a leküldéses értesítések támogatását a Xamarin.androidban.

1. Kattintson a jobb gombbal a hello összetevők mappára a Xamarin.Android-alkalmazásban, és válassza a **további összetevők beszerzése**.
2. Keresse meg a hello **Azure Messaging** összetevő, és adja hozzá toohello projekt.
3. Keresse meg a hello **Google Cloud Messaging Client** összetevő, és adja hozzá toohello projekt.

### <a name="set-up-notification-hubs-in-your-project"></a>Értesítési központok beállítása a projektben
1. Gyűjtse össze az Android alkalmazásra és az értesítési központ adatait a következő hello:
   
   * **GoogleProjectNumber**: a Projektszám értéke beszerezni az alkalmazást a Google fejlesztői portálján hello hello áttekintése. Feljegyezte korábbi ennek az értéknek hello portal hello alkalmazás létrehozásakor.
   * **Figyelési kapcsolati karakterlánc**: hello irányítópulton a hello [klasszikus Azure portál], kattintson a **kapcsolati karakterláncok megjelenítése**. Másolás hello *DefaultListenSharedAccessSignature* kapcsolati karakterláncot ezen értékhez.
   * **Központnév**: hello a központ neve hello [klasszikus Azure portál]. Például: *mynotificationhub2*.
     
     Hozzon létre egy **Constants.cs** osztályt a Xamarin-projekthez, és adja meg a következő állandó értékek hello osztályban hello. Hello helyőrzőket cserélje le az értékeket.
     
       public static class Constants   {
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       }
2. Adja hozzá hello következő using utasításokat túl**MainActivity.cs**:
   
        using Android.Util;
        using Gcm.Client;
3. Adja hozzá egy példány változó toohello `MainActivity` osztály, amely hello alkalmazás futtatásakor használt tooshow egy figyelmeztető párbeszédpanel lesz:
   
        public static MainActivity instance;
4. Hozzon létre a következő metódus a hello hello **MainActivity** osztály:
   
        private void RegisterWithGCM()
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. A hello `OnCreate` metódusában **MainActivity.cs**, hello inicializálása `instance` változó, és adjon hozzá egy túl`RegisterWithGCM`:
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from hello "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from hello layout resource,
            // and attach an event tooit
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
6. Hozzon létre az új **MyBroadcastReceiver** osztályt.
   
   > [!NOTE]
   > Alább végigvezetjük a **BroadcastReceiver** osztály létrehozásának folyamatán az alapoktól kezdve. Azonban egy másik toomanually gyors létrehozása **MyBroadcastReceiver.cs** toorefer toohello van **GcmService.cs** fájl található a hello Xamarin.Android-projekt minta hello mellékelt[NotificationHubs-mintákban][GitHub]. Másolás **GcmService.cs** és osztálynevek módosítása egy remek toostart is lehet.
   > 
   > 
7. Adja hozzá hello következő using utasításokat túl**MyBroadcastReceiver.cs** (utaló toohello összetevőre és szerelvényre, korábban hozzáadott):
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. A **MyBroadcastReceiver.cs**, adja hozzá a következő engedélyekre vonatkozó kérései között hello hello **használatával** utasítások és hello **névtér** deklarációjában:
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. A **MyBroadcastReceiver.cs**, módosítsa a hello **MyBroadcastReceiver** toomatch hello következő osztályban:
   
        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };
   
            public const string TAG = "MyBroadcastReceiver-GCM";
        }
10. A **MyBroadcastReceiver.cs** osztályban adjon hozzá egy másik, **PushHandlerService** nevű osztályt, amely a **GcmServiceBase** osztályból származik. Győződjön meg arról, hogy tooapply hello **szolgáltatás** toohello attribútumosztály:
    
         [Service] // Must use hello service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
11. A **GcmServiceBase** az **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** és **OnError()** metódust valósítja meg. A **PushHandlerService** megvalósítási osztálynak felül kell bírálnia ezeket a módszereket, és ezek a módszerek a válasz toointeracting hello értesítési központban fogja érvényesítést.
12. Bírálja felül a hello **OnRegistered()** metódus a **PushHandlerService** hello kód a következő használatával:
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "hello device has been Registered!");
    
             Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                         context);
             try
             {
                 Hub.UnregisterAll(registrationId);
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
    
             //var tags = new List<string>() { "falcons" }; // create tags if you want
             var tags = new List<string>() {};
    
             try
             {
                 var hubRegistration = Hub.Register(registrationId, tags.ToArray());
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
         }
    
    > [!NOTE]
    > A hello **OnRegistered()** fent kódját, vegye figyelembe a hello képességét toospecify címkék tooregister adott üzenetkezelési csatornák.
    > 
    > 
13. Bírálja felül a hello **OnMessage** metódus a **PushHandlerService** hello kód a következő használatával:
    
        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");
    
            var msg = new StringBuilder();
    
            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }
    
            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }
14. Adja hozzá a következő hello **createNotification** és **dialogNotify** módszerek túl**PushHandlerService** a felhasználók értesítésére értesítés fogadásakor.
    
    > [!NOTE]
    > Az Android 5.0-s és újabb verzióiban az értesítések kialakítása jelentősen eltér a korábbi verzióktól. Ha a tesztelést az Android 5.0-s vagy újabb, hello app tooreceive hello értesítésre futtató toobe kell. További információ: [Android-értesítések](http://go.microsoft.com/fwlink/?LinkId=615880).
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent tooshow UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create hello notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove hello notification once hello user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set hello notification info
            //we use hello pending intent, passing our ui intent over, which will get called
            //when hello notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show hello notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }
    
        protected void dialogNotify(String title, String message)
        {
    
            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }
15. Bírálja felül az **OnUnRegistered()**, **OnRecoverableError()** és **OnError()** absztrakt tagot a kód lefordításához:
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "hello device has been unregistered!");
        }
    
        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);
    
            return base.OnRecoverableError (context, errorId);
        }
    
        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }

## <a name="run-your-app-in-hello-emulator"></a>Az alkalmazás futtatása az emulátorban hello
Ha hello emulátorban futtatja az alkalmazást, győződjön meg arról, hogy egy Android virtuális eszközt (AVD), amely támogatja a Google API-kat használjon.

> [!IMPORTANT]
> A sorrend tooreceive leküldéses értesítések be kell állítania egy Google-fiókba az Android virtuális eszközön. (Hello emulátor, lépjen a túl**beállítások** kattintson **fiók hozzáadása**.) Győződjön meg arról, hogy hello emulátor csatlakoztatott toohello Internet.
> 
> [!NOTE]
> Az Android 5.0-s és újabb verzióiban az értesítések kialakítása jelentősen eltér a korábbi verzióktól. További információ: [Android-értesítések](http://go.microsoft.com/fwlink/?LinkId=615880).
> 
> 

1. A **Tools** (Eszközök) részen kattintson az **Open Android Emulator Manager** (Android-emulátorkezelő megnyitása) elemre, jelölje ki az eszközt, majd kattintson az **Edit** (Szerkesztés) gombra.
   
      ![][18]
2. A **Target** (Cél) értékeként válassza a **Google APIs** (Google API-k) lehetőséget, majd kattintson az **OK** gombra.
   
      ![][19]
3. A hello felső eszköztáron kattintson **futtatása**, majd válassza ki az alkalmazást. Hello emulátor elindul, és a hello alkalmazást futtat.
   
   hello alkalmazás lekéri hello *registrationId* a GCM és hello értesítési központ regisztrál.

## <a name="send-notifications-from-your-backend"></a>Értesítések küldése a háttérrendszerből
Értesítések fogadásának az alkalmazásban való értesítések a hello tesztelheti [klasszikus Azure portál] hello keresztül debug lapon hello értesítési központ, ahogy az alábbi üdvözlő képernyőt.

![][30]

A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET. Hello REST API közvetlen toosend értesítési üzenetek, ha a szalagtár nem érhető el a kiszolgáló is használható.

Egyéb oktatóprogramok találhatók, amelyeknél az értesítések küldésével felmerülhet tooreview listája itt található:

* ASP.NET: Lásd: [Notification Hubs használata toopush értesítések toousers].
* Az Azure Notification Hubs Java SDK: lásd: [hogyan toouse Notification Hubs Java](notification-hubs-java-push-notification-tutorial.md) a küldhetők értesítések a Javával. Ez az Eclipse-ben lett tesztelve Android-fejlesztéshez.
* PHP: Lásd: [hogyan toouse php-ből a Notification Hubs](notification-hubs-php-push-notification-tutorial.md).

Hello hello oktatóanyag következő alszakaszaiban értesítéseket küld egy .NET-Konzolalkalmazás használatával, és egy mobilszolgáltatás használatával egy csomópontparancsfájl segítségével.

#### <a name="optional-send-notifications-by-using-a-net-app"></a>(Választható) Értesítések küldése .NET-alkalmazás használatával
Ebben a szakaszban egy .NET-konzolalkalmazás használatával küldünk értesítéseket.

1. Hozzon létre egy új Visual C#-konzolalkalmazást:
   
      ![][20]
2. A Visual Studióban kattintson az **Eszközök**, a **NuGet Package Manager** (NuGet-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.
   
    Ez a Visual Studio hello Csomagkezelő konzol jeleníti meg.
3. Hello Package Manager Console ablakban, állítson be hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Nyissa meg hello Program.cs fájlt, és adja hozzá a következő hello `using` utasítást:
   
        using Microsoft.Azure.NotificationHubs;
5. Az a `Program` osztály, adja hozzá a következő metódus hello. Hello helyőrzők szövegét frissítése a *DefaultFullSharedAccessSignature* kapcsolati karakterláncot és a központ nevét a hello [klasszikus Azure portál].
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. Adja hozzá az alábbi hello a **fő** módszert:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Nyomja le az hello F5 kulcs toorun hello alkalmazást. Hello alkalmazásban egy értesítést kell kapnia.
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a>(Választható) Értesítések küldése mobilszolgáltatás használatával
1. Kövesse [A Mobile Services használatának első lépései] című témakör utasításait.
2. Jelentkezzen be toohello [klasszikus Azure portál], és jelölje ki a mobilszolgáltatást.
3. Jelölje be hello **Feladatütemező** hello legfelső lap.
   
      ![][22]
4. Hozzon létre egy új ütemezett feladatot, szúrjon be egy nevet, és válassza az **On demand** (Igény szerint) lehetőséget.
   
      ![][23]
5. Ha hello feladat jön létre, kattintson a hello feladat neve. Kattintson a hello **parancsfájl** hello felső sávon fülre.
6. Helyezze be a következő parancsfájlt a scheduler függvényébe hello. Győződjön meg arról, hogy tooreplace hello helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultFullSharedAccessSignature* korábban beszerzett. Kattintson a **Save** (Mentés) gombra.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );
7. Kattintson a **egyszeri futtatás** hello alsó sáv. Egy bejelentési értesítést kell kapnia.

## <a name="next-steps"></a>Következő lépések
Ez az egyszerű példában küldött értesítések tooall az Android-eszközök. A rendezés tootarget adott felhasználók, tekintse meg a toohello oktatóanyag [Notification Hubs használata toopush értesítések toousers]. Ha azt szeretné, toosegment a felhasználókat érdeklődési körök, olvasható [legfrissebb hírek Notification Hubs használata toosend]. További tudnivalók toouse értesítési központok [Notification Hubs használatával] és hello [Notification Hubs útmutató-toofor Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app toohello Notification Hub]: #connecting-app
[Run your app with hello emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[A Mobile Services használatának első lépései]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[klasszikus Azure portál]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs útmutató-toofor Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Notification Hubs használata toopush értesítések toousers]: /manage/services/notification-hubs/notify-users-aspnet
[legfrissebb hírek Notification Hubs használata toosend]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Google Cloud Messaging Client összetevő]: http://components.xamarin.com/view/GCMClient/
[Azure Messaging összetevő]: http://components.xamarin.com/view/azure-messaging
