---
title: "Az Azure Notification Hubs használatának első lépései Kindle-alkalmazásokhoz | Microsoft Docs"
description: "Ebből az oktatóanyagból elsajátíthatja, hogy miként használható az Azure Notification Hubs leküldéses értesítések küldéséhez Kindle-alkalmazások esetén."
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
ms.openlocfilehash: 7206f152ed7270abc62536a9ee164f7227833bcc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Ismerkedés a Notification Hubs szolgáltatással Kindle-alkalmazásokhoz
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
Ebből az oktatóanyagból elsajátíthatja, hogy miként használható az Azure Notification Hubs leküldéses értesítések küldéséhez Kindle-alkalmazások esetén.
Létre fog hozni egy üres Kindle-alkalmazást, amely leküldéses értesítéseket fogad az Amazon Device Messaging (ADM) használatával.

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyaghoz az alábbiakra lesz szükség:

* Töltse le az Android SDK-t (amennyiben Eclipse-t használ) az <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android webhelyéről</a>.
* Kövesse a <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Setting Up Your Development Environment</a> (A fejlesztési környezet beállítása) című cikk lépéseit a Kindle fejlesztési környezetének beállításához.

## <a name="add-a-new-app-to-the-developer-portal"></a>Új fájl hozzáadása a fejlesztői portálhoz
1. Először hozzon létre egy alkalmazást az [Amazon fejlesztői portálján].
   
    ![][0]
2. Jegyezze fel az alkalmazáskulcsot (**Application Key**).
   
    ![][1]
3. A portálon kattintson az alkalmazása nevére, majd a **Device Messaging** (Eszköz üzenetkezelése) lapra.
   
    ![][2]
4. Kattintson a **Create a New Security Profile** (Új biztonsági profil létrehozása) elemre, hozzon létre egy új biztonsági profilt (például: **TestAdm security profile**). Ezután kattintson a **Save** (Mentés) gombra.
   
    ![][3]
5. Kattintson a **Security Profiles** (Biztonsági profilok) lapra az imént létrehozott biztonsági profil megtekintéséhez. Jegyezze fel a **Client ID** (Ügyfél-azonosító) és a **Client Secret** (Titkos ügyfélkulcs) értékeket, mert ezekre később szükség lesz.
   
    ![][4]

## <a name="create-an-api-key"></a>API-kulcs létrehozása
1. Nyisson meg egy parancssort rendszergazdai jogosultságokkal.
2. Lépjen az Android SDK mappába.
3. Írja be a következő parancsot:
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. A kulcstár (**keystore**) jelszavához az **android** szót adja meg.
5. Jegyezze fel az **MD5**-ujjlenyomatot.
6. A fejlesztői portálra visszatérve kattintson az **Android/Kindle** lehetőségre a **Messaging** (Üzenetküldés) lapon, és adja meg az alkalmazásához használt csomag nevét (például: **com.sample.notificationhubtest**), valamint az **MD5** értékét, majd kattintson a **Generate API Key** (API-kulcs létrehozása) lehetőségre.

## <a name="add-credentials-to-the-hub"></a>Hitelesítő adatok hozzáadása a központhoz
A portálon adja hozzá a titkos ügyfélkulcsot és az ügyfél-azonosítót az értesítési központ **Configure** (Konfigurálás) lapján.

## <a name="set-up-your-application"></a>Az alkalmazás beállítása
> [!NOTE]
> Alkalmazás létrehozásakor legalább 17-es API-szintet használjon.
> 
> 

Az ADM-kódtárak hozzáadása az Eclipse-projekthez:

1. Az ADM-kódtár beszerzéséhez [töltse le az SDK-t]. Csomagolja ki az SDK zip-fájlját.
2. Az Eclipse-ben kattintson a jobb gombbal a projektjére, majd kattintson a **Properties** (Tulajdonságok) elemre. A bal oldalon válassza a **Java Build Path** (Java-verzió elérési útja) lehetőséget, majd a fent található **Libraries **(Kódtárak) lapot. Kattintson az **Add External Jar** (Külső jar-fájl hozzáadása) lehetőségre, és válassza az `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` fájlt abból a könyvtárból, ahová kicsomagolta az Amazon SDK-t.
3. Töltse le a NotificationHubs Android SDK-t (hivatkozás).
4. Tömörítse ki a csomagot, aztán húzza a `notification-hubs-sdk.jar` fájlt az Eclipse `libs` mappájába.

Az alkalmazás jegyzékének módosítása az ADM támogatásához:

1. Adja hozzá a következő Amazon-névteret a gyökérjegyzék-elemhez:

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. A jegyzékelem első elemeként adja hozzá az engedélyeket. A **[YOUR PACKAGE NAME]** kifejezés helyére helyettesítse be az alkalmazás létrehozásához használt csomagot.
   
        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />
   
        <uses-permission android:name="android.permission.INTERNET"/>
   
        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />
   
        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />
   
        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />
2. Szúrja be a következő elemet az alkalmazáselem első gyermekeként. Ne felejtse el a **[YOUR SERVICE NAME]** kifejezés helyére beírni a következő szakaszban (a csomaggal együtt) létrehozandó ADM üzenetkezelő nevét, a **[YOUR PACKAGE NAME]** kifejezés helyére pedig az alkalmazás létrehozására használt csomag nevét.
   
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
   
            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
   
          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Az ADM üzenetkezelőjének létrehozása
1. Hozzon létre egy új osztályt, mely a `com.amazon.device.messaging.ADMMessageHandlerBase` elemtől öröklődik, és nevezze el `MyADMMessageHandler`-nek, ahogy azt a következő ábra is mutatja:
   
    ![][6]
2. Adja hozzá a következő `import`-utasításokat:
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. Adja hozzá a következő kódot a létrehozott osztályhoz. Ne felejtse el behelyettesíteni a központ nevét és a kapcsolati karakterláncot (figyelés):
   
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
4. Adja hozzá a következő kódot az `OnMessage()` metódushoz:
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. Adja hozzá a következő kódot az `OnRegistered` metódushoz:
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. Adja hozzá a következő kódot az `OnUnregistered` metódushoz:
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. A `MainActivity` metódushoz adja hozzá a következő importálási utasítást:
   
        import com.amazon.device.messaging.ADM;
8. Adja hozzá a következő kódot az `OnCreate` metódus végéhez:
   
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

## <a name="add-your-api-key-to-your-app"></a>Az API-kulcs hozzáadása az alkalmazáshoz
1. Az Eclipse-ben hozzon létre egy új fájlt **api_key.txt** néven a projekt könyvtáreszközeiben.
2. Nyissa meg a fájlt, és másolja be az Amazon fejlesztői portálon létrehozott API-kulcsot.

## <a name="run-the-app"></a>Az alkalmazás futtatása
1. Indítsa el az emulátort.
2. Az emulátorban pöccintsen lefelé a képernyő tetejéről, és kattintson a **Settings** (Beállítások), majd a **My account** (Saját fiók) elemre, végül pedig jelentkezzen be egy érvényes Amazon-fiókkal.
3. Futtassa az alkalmazást az Eclipse-ben.

> [!NOTE]
> Ha probléma merül fel, ellenőrizze az emulátor idejét (vagy az eszközét). Az idő értékének pontosnak kell lennie. A Kindle-emulátor idejének módosításához futtassa a következő parancsot az Android SDK platformeszközök könyvtárából:
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Üzenet küldése
Üzenet küldése a .NET használatával:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon fejlesztői portálján]: https://developer.amazon.com/home.html
[töltse le az SDK-t]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
