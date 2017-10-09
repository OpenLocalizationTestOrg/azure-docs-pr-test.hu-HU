---
title: "aaaGet lépések az Azure Notification Hubs szolgáltatással Kindle-alkalmazásokhoz |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Kindle-alkalmazások."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 7c28d64372cd2d90bab9cd9bf818d333f3478f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Ismerkedés a Notification Hubs szolgáltatással Kindle-alkalmazásokhoz
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Kindle-alkalmazások.
Létre fog hozni egy üres Kindle-alkalmazást, amely leküldéses értesítéseket fogad az Amazon Device Messaging (ADM) használatával.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag hello következő szükséges:

* Android SDK (feltételezzük, hogy Eclipse-et használ) hello beszerezni hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android webhelyéről</a>.
* Hello kövesse <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">beállítás be a fejlesztési környezet</a> tooset Kindle fejlesztési környezet.

## <a name="add-a-new-app-toohello-developer-portal"></a>Adja hozzá egy új alkalmazás toohello fejlesztői portálján
1. Először hozzon létre egy alkalmazást hello [Amazon fejlesztői portálján].
   
    ![][0]
2. Másolás hello **Alkalmazáskulcsot**.
   
    ![][1]
3. Hello portálon, kattintson az alkalmazás hello nevére, és kattintson a hello **Device Messaging** fülre.
   
    ![][2]
4. Kattintson a **Create a New Security Profile** (Új biztonsági profil létrehozása) elemre, hozzon létre egy új biztonsági profilt (például: **TestAdm security profile**). Ezután kattintson a **Save** (Mentés) gombra.
   
    ![][3]
5. Kattintson a **biztonsági profilok** tooview hello biztonsági profil, most hozott létre. Másolás hello **ügyfél-azonosító** és **Ügyfélkulcs** értékek későbbi használatra.
   
    ![][4]

## <a name="create-an-api-key"></a>API-kulcs létrehozása
1. Nyisson meg egy parancssort rendszergazdai jogosultságokkal.
2. Keresse meg a toohello Android SDK mappába.
3. Adja meg a következő parancs hello:
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. A hello **keystore** jelszót, írja be **android**.
5. Másolás hello **MD5** ujjlenyomat.
6. Vissza a hello fejlesztői portálján hello **Messaging** lapra, majd **Android/Kindle** , és írja be az alkalmazás hello hello csomag nevét (például **com.sample.notificationhubtest**) és hello **MD5** értékét, és kattintson a **API-kulcs létrehozása**.

## <a name="add-credentials-toohello-hub"></a>Hitelesítő adatok toohello hub hozzáadása
Hello portálon, vegye fel a hello ügyfél titkos kulcs és az ügyfél azonosító toohello **konfigurálása** az értesítési központ lapján.

## <a name="set-up-your-application"></a>Az alkalmazás beállítása
> [!NOTE]
> Alkalmazás létrehozásakor legalább 17-es API-szintet használjon.
> 
> 

Hello ADM szalagtárak tooyour Eclipse-projekt hozzáadása:

1. tooobtain hello ADM-kódtár [hello SDK letöltése]. Bontsa ki a hello SDK zip-fájl.
2. Az Eclipse-ben kattintson a jobb gombbal a projektjére, majd kattintson a **Properties** (Tulajdonságok) elemre. Válassza ki **Java Build elérési** hello maradt, és jelölje ki hello ** szalagtárak ** hello felső fülre. Kattintson a **külső Jar hozzáadása**, és jelölje be hello fájl `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` hello könyvtárból, ahová kicsomagolta hello Amazon SDK-t.
3. Töltse le a NotificationHubs Android SDK-t (hivatkozás) hello.
4. Bontsa ki a hello csomagot, és húzza a hello fájl `notification-hubs-sdk.jar` történő hello `libs` mappa az eclipse-ben.

Az alkalmazás jegyzékének toosupport ADM szerkesztése:

1. Adja hozzá hello Amazon-névteret hello gyökérjegyzék-elemhez:

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. Engedélyek hozzáadása hello első eleme hello jegyzék elem alatt. Helyettesítő **[YOUR PACKAGE NAME]** hello csomaggal, amellyel toocreate az alkalmazást.
   
        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />
   
        <uses-permission android:name="android.permission.INTERNET"/>
   
        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />
   
        <!-- This permission allows your app access tooreceive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />
   
        <!-- ADM uses WAKE_LOCK tookeep hello processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />
2. Helyezze be a hello elem követően hello hello alkalmazáselem első gyermekeként. Ne feledje toosubstitute **[YOUR SERVICE NAME]** hello nevű az ADM üzenetkezelőjének létrehozása a következő szakasz hello (többek között a következőket hello csomag), és cserélje le a **[YOUR PACKAGE NAME]** a hello az alkalmazást létrehozó csomag neve.
   
        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />
   
        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />
   
            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >
   
            <!-- toointeract with ADM, your app must listen for hello following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
   
          <!-- Replace hello name in hello category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Az ADM üzenetkezelőjének létrehozása
1. Hozzon létre egy új osztályt, amely örökli `com.amazon.device.messaging.ADMMessageHandlerBase` és adjon neki nevet `MyADMMessageHandler`, ahogy az ábra a következő hello:
   
    ![][6]
2. Adja hozzá a következő hello `import` utasításokat:
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. Adja hozzá a következő kódot a létrehozott osztályhoz hello hello. Ne feledje toosubstitute hello központ nevét és a kapcsolati karakterláncot (Figyelés):
   
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
          private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }
   
        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }
   
            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }
   
            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();
   
                mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);
   
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                  new Intent(ctx, MainActivity.class), 0);
   
            NotificationCompat.Builder mBuilder =
                  new NotificationCompat.Builder(ctx)
                  .setSmallIcon(R.mipmap.ic_launcher)
                  .setContentTitle("Notification Hub Demo")
                  .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                  .setContentText(msg);
   
             mBuilder.setContentIntent(contentIntent);
             mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }
4. Adja hozzá a következő kód toohello hello `OnMessage()` módszert:
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. Adja hozzá a következő kód toohello hello `OnRegistered` módszert:
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. Adja hozzá a következő kód toohello hello `OnUnregistered` módszert:
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. A hello `MainActivity` módszer, adja hozzá a következő importálási utasítást hello:
   
        import com.amazon.device.messaging.ADM;
8. Adja hozzá a következő kód hello hello végén hello `OnCreate` módszert:
   
        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                         MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-tooyour-app"></a>Az API-kulcs tooyour alkalmazás hozzáadása
1. Az eclipse-ben hozzon létre egy új fájlt **api_key.txt** a hello a projekt könyvtáreszközeiben.
2. Nyissa meg a hello fájlt, és másolja hello API-kulcs hello Amazon fejlesztői portálon létrehozott.

## <a name="run-hello-app"></a>Hello alkalmazás futtatása
1. Indítsa el a hello emulátor.
2. Hello emulátorban, csúsztassa az ujját hello fentről, és kattintson a **beállítások**, és kattintson a **fiókomat** és egy érvényes Amazon-fiókot regisztrálni.
3. Az eclipse-ben futtassa a hello alkalmazást.

> [!NOTE]
> Ha a probléma akkor fordul elő, ellenőrizze a hello idő hello emulátor (vagy egy eszköz). hello idő értékének pontosnak kell lennie. hello Kindle-emulátor toochange hello ideje, futtathatja a következő parancsot az Android SDK platformeszközök könyvtárából hello:
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Üzenet küldése
toosend üzenet .NET használatával:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon fejlesztői portálján]: https://developer.amazon.com/home.html
[hello SDK letöltése]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
