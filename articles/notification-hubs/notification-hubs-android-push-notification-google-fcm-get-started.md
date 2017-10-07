---
title: "aaaSending leküldéses értesítések tooAndroid az Azure Notification Hubs és a Firebase Cloud Messaging |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs és a Firebase Cloud Messaging toopush értesítések tooAndroid eszközök."
services: notification-hubs
documentationcenter: android
keywords: "leküldéses értesítések,leküldéses értesítés,android leküldéses értesítés,fcm,firebase cloud messaging"
author: ysxu
manager: erikre
editor: 
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/14/2016
ms.author: yuaxu
ms.openlocfilehash: d2e57437ac7b0ef77abf048f991043620621e58d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooandroid-with-azure-notification-hubs"></a>Az Azure Notification Hubs leküldéses értesítések tooAndroid küldése
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Áttekintés
> [!IMPORTANT]
> Ez a témakör a leküldéses értesítések Google Firebase Cloud Messaging (FCM) használatával történő küldését mutatja be. Ha a Google Cloud Messaging (GCM) továbbra is használ, tekintse meg [küldő leküldéses értesítések tooAndroid az Azure Notification Hubs és a GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).
> 
> 

Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs és Firebase Cloud Messaging toosend leküldéses értesítések tooan Android-alkalmazás.
Létre fog hozni egy üres Android-alkalmazást, amely leküldéses értesítéseket fogad a Firebase Cloud Messaging (FCM) használatával.

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

az oktatóanyag kódjának befejeződött hello letölthető a GitHub [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).

## <a name="prerequisites"></a>Előfeltételek
> [!IMPORTANT]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).
> 
> 

* Továbbá tooan aktív Azure-fiók említettük, ez az oktatóanyag hello legújabb verziója szükséges [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).
* Android 2.3 vagy újabb a Firebase Cloud Messaging esetében.
* A Google-tárház 27-es vagy újabb verziója a Firebase Cloud Messaging esetében.
* A Google Play-szolgáltatások 9.0.2-es vagy újabb verziója a Firebase Cloud Messaging esetében.
* Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Android-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.

## <a name="create-a-new-android-studio-project"></a>Új Android Studio-projekt létrehozása
1. Az Android Studióban indítson el egy új Android Studio-projektet.
   
       ![Android Studio - new project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Válassza ki a hello **telefon és táblagép** tényező és hello **minimális SDK** , amelyet az toosupport. Ezután kattintson a **Next** (Tovább) gombra.
   
       ![Android Studio - project creation workflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Válasszon **üres tevékenység** hello fő tevékenység, kattintson a **következő**, és kattintson a **Befejezés**.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>A Firebase Cloud Messaginget támogató projekt létrehozása
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a>Új értesítési központ konfigurálása
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. A hello **beállítások** az értesítési központ, jelölje be a panelt **értesítési szolgáltatások** , majd **Google (GCM)**. Adja meg a korábban kimásolt a hello hello FCM kiszolgálókulcs [Firebase konzol](https://firebase.google.com/console/) kattintson **mentése**.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Az értesítési központ most Firebase felhő Messagin a konfigurált toowork, és van hello kapcsolati karakterláncok tooboth regisztrálja az alkalmazást tooreceive, valamint leküldéses értesítések küldéséhez.

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello értesítési központ
### <a name="add-google-play-services-toohello-project"></a>Google Play services toohello projekt hozzáadása
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Azure Notification Hubs-kódtárak felvétele
1. A hello `Build.Gradle` hello fájlt **app**, adja hozzá a következő sorokat hello hello **függőségek** szakasz.
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. Adja hozzá a következő tárházat után hello hello **függőségek** szakasz.
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-hello-androidmanifestxml"></a>Hello AndroidManifest.xml frissítése.
1. toosupport FCM, létre kell hoznunk egy Példányazonosító-figyelő szolgáltatást a kódban, amely túl[regisztrációs jogkivonatok lekérésére](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) használatával [Google FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). Az oktatóanyag azt hello osztály nevet `MyInstanceIDService`. 
   
    Adja hozzá a következő szolgáltatás definíciós toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke. 
   
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
2. Miután a FCM regisztrációs jogkivonat a hello FirebaseInstanceId API, ezzel túl[hello Azure Notification Hub regisztrálása](notification-hubs-push-notification-registration-management.md). Ez a regisztráció támogatjuk hello háttér használatával egy `IntentService` nevű `RegistrationIntentService`. Ez a szolgáltatás az FCM regisztrációs jogkivonat frissítését is el fogja végezni.
   
    Adja hozzá a következő szolgáltatás definíciós toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke. 
   
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
3. A fogadó tooreceive értesítéseket is meghatározunk. Adja hozzá a következő fogadó definition toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke. Cserélje le a hello `<your package>` helyőrzőt hello hello hello tetején látható tényleges csomagnévre `AndroidManifest.xml` fájlt.
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. Adja hozzá a következő szükséges FCM hello kapcsolatos alábbi hello engedélyek `</application>` címke. Győződjön meg arról, hogy tooreplace `<your package>` hello hello tetején látható hello csomag nevű `AndroidManifest.xml` fájlt.
   
    Ezekről az engedélyekről további információkért lásd: [GCM-ügyfélalkalmazás beállítása androidhoz](https://developers.google.com/cloud-messaging/android/client#manifest) és [telepítse át a GCM-ügyfélalkalmazás az Android tooFirebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

### <a name="adding-code"></a>Kód felvétele
1. A projekt nézetben hello, bontsa ki a **app** > **src** > **fő** > **java**. Kattintson a jobb gombbal a **java** területen látható csomagmappára, kattintson a **New** (Új), majd a **Java Class** (Java-osztály) elemre. Adjon hozzá egy új, `NotificationSettings` nevű osztályt. 
   
    ![Android Studio – új Java-osztály](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    Ellenőrizze, hogy tooupdate hello ezek hello hello kódját a következő három helyőrzőt `NotificationSettings` osztály:
   
   * **SenderId**: hello korábban beolvasott Küldőazonosító hello **Cloud Messaging** szereplő hello projekt beállítások lapon [Firebase konzol](https://firebase.google.com/console/).
   * **HubListenConnectionString**: hello **DefaultListenAccessSignature** kapcsolati karakterlánca. Kapcsolati karakterlánc másolhatja kattintva **hozzáférési házirendek** a hello **beállítások** hello központ paneljén [Azure Portal].
   * **HubName**: hello név használata hello hello központ paneljén megjelenő értesítési központ [Azure Portal].
     
     `NotificationSettings` kód:
     
       public class NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       }
2. Használatával a hello fent leírt lépésekkel, vegyen fel egy másik új osztályt `MyInstanceIDService`. Ez lesz az általunk megvalósított Példányazonosító figyelőszolgáltatás.
   
    Ez az osztály kódját hello szolgáltatás fel fogja hívni a `IntentService` túl[frissítési hello FCM jogkivonat](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) hello háttérben.
   
        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;

        public class MyInstanceIDService extends FirebaseInstanceIdService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.d(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


1. Adja hozzá egy másik új osztály tooyour nevű projekt, `RegistrationIntentService`. Ez lesz a hello megvalósítását a `IntentService` , amely elvégzi [frissítés hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) és [hello értesítési központ regisztrálása](notification-hubs-push-notification-registration-management.md).
   
    Ez az osztály kódját a következő hello használata.
   
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
   
        public class RegistrationIntentService extends IntentService {
   
            private static final String TAG = "RegIntentService";
   
            private NotificationHub hub;
   
            public RegistrationIntentService() {
                super(TAG);
            }
   
            @Override
            protected void onHandleIntent(Intent intent) {
   
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
   
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
   
                    // Storing hello registration id that indicates whether hello generated token has been
                    // sent tooyour server. If it is not stored, send hello token tooyour server,
                    // otherwise your server should have already received hello token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    // Check if hello token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
   
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
   
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
   
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed toocomplete registration", e);
                    // If an exception happens while fetching hello new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt hello update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. Az a `MainActivity` osztály, adja hozzá a következő hello `import` fent hello utasítások osztály deklarációjában.
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. Adja hozzá az alábbi privát tagokat hello osztály hello tetején hello. Ezek használjuk [Google Play-szolgáltatások hello elérhetőségének ellenőrzése a Google által javasolt módon](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. Az a `MainActivity` osztály, adja hozzá a következő metódus toohello Google Play-szolgáltatások rendelkezésre állásának hello. 
   
        /**
         * Check hello device toomake sure it has hello Google Play Services APK. If
         * it doesn't, display a dialog that allows users toodownload hello APK from
         * hello Google Play Store or enable it in hello device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }
5. Az a `MainActivity` osztály, adja hozzá a következő kódot, amellyel a Google Play-szolgáltatások hívása előtt hello a `IntentService` tooget a FCM regisztrációs jogkivonat és regisztrálása az értesítési központban.
   
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService tooregister this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. A hello `OnCreate` hello metódusában `MainActivity` osztály, adja hozzá a hello tevékenység létrehozásakor a következő kód toostart hello regisztrációs folyamat során.
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. Adja hozzá a további módszereket toohello `MainActivity` tooverify alkalmazások állapotának és a jelentés az alkalmazás állapotát.
   
        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
   
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
   
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
   
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
   
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }
8. Hello `ToastNotify` metódusnak hello *"Hello World"* `TextView` tooreport állapot és az értesítések folyamatos jelentéséhez az hello app szabályozzák. Az activity_main.xml elrendezésben vegye fel az adott vezérlő azonosítója a következő hello.
   
       android:id="@+id/text_hello"
9. A Tovább gombra a fogadó hello AndroidManifest.xml a meghatározott a adunk hozzá egy alosztályt. Adja hozzá egy másik új osztály tooyour nevű projekt `MyHandler`.
10. Adja hozzá a következő importálási utasításokat a felső hello hello `MyHandler.java`:
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. Adja hozzá a következő kódot a hello hello `MyHandler` osztály, így a alosztálya `com.microsoft.windowsazure.notifications.NotificationsHandler`.
    
    Ez a kód felülírja hello `OnReceive` metódust, így hello kezelő jelentést küld a kapott értesítésekről. hello értesítéskezelőnek is elküldi a hello leküldéses értesítési toohello Android értesítéskezelőjén hello segítségével `sendNotification()` metódust. Hello `sendNotification()` metódust akkor kell végrehajtani, amikor hello alkalmazás nem fut, és értesítés érkezik.
    
        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
    
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
    
            private void sendNotification(String msg) {
    
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
    
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
    
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }
12. Az Android Studióban hello menüsávjában kattintson **Build** > **Rebuild Project** toomake meg arról, hogy a kód a jelen-e hibák.
13. Hello alkalmazás futtatható az eszközön, és ellenőrizze, hogy sikeresen hello értesítési központban regisztrálja. 
    
    > [!NOTE]
    > Regisztráció sikertelen hello kezdeti indítási amíg hello `onTokenRefresh()` módszer azonosító service-példány neve. hello frissítési kell kezdeményezhet sikeres regisztráció hello értesítési központban.
    > 
    > 

## <a name="sending-push-notifications"></a>Leküldéses értesítések küldése
Leküldéses értesítések fogadásának az alkalmazásban keresztül hello elküldésével tesztelheti [Azure Portal] -hello keressen **hibaelhárítás** hello központ paneljén szakasz alább látható módon.

![Azure Notification Hubs – küldés tesztelése](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-hello-app"></a>(Választható) Leküldéses értesítések küldéséhez közvetlenül hello alkalmazásból
> [!IMPORTANT]
> Ebben a példában a hello ügyfélalkalmazás értesítések küldésére szolgáló jelleggel valósul meg. Mivel ezt a beállítást hello `DefaultFullSharedAccessSignature` toobe hello ügyfélalkalmazás megtalálható, azt mutatja meg az értesítési központ toohello kockázata, hogy a felhasználó szerezhetnek hozzáférés nem engedélyezett toosend értesítések tooyour ügyfelek.
> 
> 

Az értesítések elküldése általában háttérkiszolgáló használatával történik. Bizonyos esetekben előfordulhat, hogy toobe képes toosend leküldéses értesítések közvetlenül ügyfélalkalmazástól hello keresi. Ez a szakasz ismerteti, hogyan toosend értesítések hello ügyfélről hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. Az Android Studio projektnézetében bontsa ki a következőt: **App** > **src** > **main** > **res** > **layout**. Nyissa meg hello `activity_main.xml` elrendezés fájlra, majd kattintson a hello **szöveg** tooupdate hello szöveg hello fájl tartalma fülre. Frissítse hello kódot, amely hozzáadja az új `Button` és `EditText` küldési vezérlők leküldéses értesítési üzenetek toohello értesítési központot. Mielőtt csak hello lap alján, adja hozzá ezt a kódot `</RelativeLayout>`.
   
        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />
   
        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />
2. Az Android Studio projektnézetében bontsa ki a következőt: **App** > **src** > **main** > **res** > **values**. Nyissa meg hello `strings.xml` fájlt, és új hello által hivatkozott hello karakterlánc-értékek `Button` és `EditText` szabályozza. Adja hozzá ezek hello fájl hello alján csak előtt `</resources>`.
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. Az a `NotificationSetting.java` fájlt, adja hozzá a következő beállítás toohello hello `NotificationSettings` osztály.
   
    Frissítés `HubFullAccess` a hello **DefaultFullSharedAccessSignature** kapcsolati karakterlánca. Ez a kapcsolati karakterlánc átmásolhatók a hello [Azure Portal] kattintva **hozzáférési házirendek** a hello **beállítások** az értesítési központ paneljén.
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";
4. Az a `MainActivity.java` fájlt, adja hozzá a következő hello `import` fent hello utasítások `MainActivity` osztály.
   
        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;
5. Az a `MainActivity.java` fájlt, adja hozzá a következő hello hello tetején a tagok hello `MainActivity` osztály.    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. Létre kell hoznia egy szoftverfrissítési hozzáférésű Jogosultságkód (SaS-) token tooauthenticate egy POST kérést toosend üzenetek tooyour értesítési központot. Ehhez hello hello kapcsolati karakterlánc legfontosabb adatainak elemzése és SaS-jogkivonatot, majd hozza létre hello, ahogyan az hello [általánosan használt fogalmakat ismertető](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API-referenciában. a következő kód hello egy megvalósítási példát szemléltet.
   
    A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály tooparse a kapcsolati karakterláncot.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * tooparse hello connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be hello DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
   
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }
7. A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály toocreate egy SaS hitelesítési jogkivonat.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from hello access key tooauthenticate a request.
         *
         * @param uri hello unencoded resource URI string for this operation. hello resource
         *            URI is hello full URI of hello Service Bus resource toowhich access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
   
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
   
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
   
                // Get an hmac_sha1 key from hello raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
   
                // Compute hello hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
   
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
   
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
   
            return token;
        }
8. A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály toohandle hello **értesítés küldése** gomb és üzenet toohello központ használatával hello beépített REST API hello leküldéses értesítést küldeni.
   
        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added toohello Authorization header on hello POST request toothe
         * notification hub. hello text in hello editTextNotificationMessage control
         * is added as hello JSON body for hello request tooadd a GCM message toohello hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
   
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
   
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
   
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
   
                            // Authenticate hello POST request with hello SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
   
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
   
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //        "tag1 || tag2 || tag3");
   
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
   
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }
   
                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }

## <a name="testing-your-app"></a>Az alkalmazás tesztelése
#### <a name="push-notifications-in-hello-emulator"></a>Leküldéses értesítések emulátorban hello
Ha azt szeretné, hogy tootest leküldéses értesítések emulátorban, győződjön meg arról, hogy az emulátor rendszerképe támogatja-e a hello Google API-szintet az alkalmazás számára is választott. Ha a lemezkép nem támogatja a natív Google API-k, akkor rendszer végül hello **szolgáltatás\_nem\_elérhető** kivétel.

Ezenkívül toohello fenti, győződjön meg arról, hogy hozzáadta a Google-fiók az emulátor futtató tooyour **beállítások** > **fiókok**. Ellenkező esetben a GCM-mel kísérletek tooregister hello eredményezhet **hitelesítési\_sikertelen** kivétel.

#### <a name="running-hello-application"></a>Hello alkalmazást futtat
1. Hello alkalmazás futtatását, és figyelje meg, hogy sikeres regisztráció jelentett-e hello regisztrációs Azonosítót.
   
       ![Testing on Android - Channel registration](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Adjon meg egy értesítési üzenetet toobe tooall Android-eszközök hello központon regisztrált küldött.
   
       ![Testing on Android - sending a message](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Nyomja le a **Send Notification** (Értesítés küldése) gombot. Hello app futó összes eszközön megjelenik egy `AlertDialog` példány hello leküldéses értesítési üzenettel. A leküldéses értesítések, amelyek nem rendelkeznek hello alkalmazás fut, de korábban már regisztrált eszközök az Android Értesítéskezelőjén hello egy értesítést fog kapni. Azok az hello bal felső sarokból lefelé pöccintve tekinthetők meg.
   
       ![Testing on Android - notifications](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Következő lépések
Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers] oktatóanyagot hello következő lépésre. Azt láthatja, hogyan toosend értesítést kapnak az ASP.NET háttérkiszolgáló használatával címkéket tootarget adott felhasználókra.

Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, tekintse meg a hello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag.

toolearn Notification Hubs további általános információt talál a [Notification Hubs használatával].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs használatával]: notification-hubs-push-notification-overview.md
[Notification Hubs használata toopush értesítések toousers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[legfrissebb hírek Notification Hubs használata toosend]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
