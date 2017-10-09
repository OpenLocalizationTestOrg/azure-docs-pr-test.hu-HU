1. Az a **app** projektet, nyissa meg hello fájl `AndroidManifest.xml`. Cserélje le a következő két lépésben hello hello kódban  *`**my_app_package**`*  nevű hello hello alkalmazáscsomag a projekthez. Ez az érték hello hello `package` hello attribútumának `manifest` címke.
2. Adja hozzá az alábbi új engedélyek után hello meglévő hello `uses-permission` elem:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. Adja hozzá a következő kód után hello hello `application` nyitó:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. Nyissa meg hello fájl *ToDoActivity.java*, és adja hozzá a következő importálási utasítást hello:

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. Adja hozzá a titkos változó toohello osztály a következő hello. Cserélje le  *`<PROJECT_NUMBER>`*  Google tooyour alkalmazást az előző eljárás hello által hozzárendelt hello projekt számával.

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. Hello definíciójának módosítása *MobileServiceClient* a **titkos** túl**nyilvános, statikus**, így most néz ki:

        public static MobileServiceClient mClient;
7. Adjon hozzá egy új osztályt toohandle értesítések. A Project Explorer nyissa meg a hello **src** > **fő** > **java** csomópontokat, és kattintson a jobb gombbal hello csomag neve csomópont. Kattintson a **új**, és kattintson a **Java-osztály**.
8. A **neve**, típus `MyHandler`, és kattintson a **OK**.

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. Hello MyHandler fájlban cserélje le a hello osztálydeklaráció:

        public class MyHandler extends NotificationsHandler {
10. Adja hozzá a következő importálási utasításokat a hello hello `MyHandler` osztály:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. Ezután adja hozzá a tag toohello `MyHandler` osztály:

        public static final int NOTIFICATION_ID = 1;
12. A hello `MyHandler` osztály, adja hozzá a következő kód toooverride hello hello **onRegistered** metódus, amely hello mobilszolgáltatás értesítési központ regisztrálja az eszközt.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
           super.onRegistered(context, gcmRegistrationId);

           new AsyncTask<Void, Void, Void>() {

               protected Void doInBackground(Void... params) {
                   try {
                       ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                       return null;
                   }
                   catch(Exception e) {
                       // handle error                
                   }
                   return null;              
               }
           }.execute();
       }
13. A hello `MyHandler` osztály, adja hozzá a következő kód toooverride hello hello **onReceive** metódus, amely hello értesítési toodisplay okoz érkezik.

        @Override
        public void onReceive(Context context, Bundle bundle) {
               String msg = bundle.getString("message");

               PendingIntent contentIntent = PendingIntent.getActivity(context,
                       0, // requestCode
                       new Intent(context, ToDoActivity.class),
                       0); // flags

               Notification notification = new NotificationCompat.Builder(context)
                       .setSmallIcon(R.drawable.ic_launcher)
                       .setContentTitle("Notification Hub Demo")
                       .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                       .setContentText(msg)
                       .setContentIntent(contentIntent)
                       .build();

               NotificationManager notificationManager = (NotificationManager)
                       context.getSystemService(Context.NOTIFICATION_SERVICE);
               notificationManager.notify(NOTIFICATION_ID, notification);
       }
14. Vissza az hello TodoActivity.java fájlban frissítse a hello **onCreate** hello metódusában *ToDoActivity* tooregister hello értesítési kezelő osztály osztályban. Győződjön meg arról, hogy tooadd Ez a kód után hello *MobileServiceClient* létrejön.

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Az alkalmazás már frissített toosupport leküldéses értesítéseket.
