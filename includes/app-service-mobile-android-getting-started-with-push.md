1. <span data-ttu-id="302bc-101">Az a **app** projektet, nyissa meg hello fájl `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="302bc-101">In your **app** project, open hello file `AndroidManifest.xml`.</span></span> <span data-ttu-id="302bc-102">Cserélje le a következő két lépésben hello hello kódban  *`**my_app_package**`*  nevű hello hello alkalmazáscsomag a projekthez.</span><span class="sxs-lookup"><span data-stu-id="302bc-102">In hello code in hello next two steps, replace *`**my_app_package**`* with hello name of hello app package for your project.</span></span> <span data-ttu-id="302bc-103">Ez az érték hello hello `package` hello attribútumának `manifest` címke.</span><span class="sxs-lookup"><span data-stu-id="302bc-103">This is hello value of hello `package` attribute of hello `manifest` tag.</span></span>
2. <span data-ttu-id="302bc-104">Adja hozzá az alábbi új engedélyek után hello meglévő hello `uses-permission` elem:</span><span class="sxs-lookup"><span data-stu-id="302bc-104">Add hello following new permissions after hello existing `uses-permission` element:</span></span>

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. <span data-ttu-id="302bc-105">Adja hozzá a következő kód után hello hello `application` nyitó:</span><span class="sxs-lookup"><span data-stu-id="302bc-105">Add hello following code after hello `application` opening tag:</span></span>

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="302bc-106">Nyissa meg hello fájl *ToDoActivity.java*, és adja hozzá a következő importálási utasítást hello:</span><span class="sxs-lookup"><span data-stu-id="302bc-106">Open hello file *ToDoActivity.java*, and add hello following import statement:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. <span data-ttu-id="302bc-107">Adja hozzá a titkos változó toohello osztály a következő hello.</span><span class="sxs-lookup"><span data-stu-id="302bc-107">Add hello following private variable toohello class.</span></span> <span data-ttu-id="302bc-108">Cserélje le  *`<PROJECT_NUMBER>`*  Google tooyour alkalmazást az előző eljárás hello által hozzárendelt hello projekt számával.</span><span class="sxs-lookup"><span data-stu-id="302bc-108">Replace *`<PROJECT_NUMBER>`* with hello project number assigned by Google tooyour app in hello preceding procedure.</span></span>

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. <span data-ttu-id="302bc-109">Hello definíciójának módosítása *MobileServiceClient* a **titkos** túl**nyilvános, statikus**, így most néz ki:</span><span class="sxs-lookup"><span data-stu-id="302bc-109">Change hello definition of *MobileServiceClient* from **private** too**public static**, so it now looks like this:</span></span>

        public static MobileServiceClient mClient;
7. <span data-ttu-id="302bc-110">Adjon hozzá egy új osztályt toohandle értesítések.</span><span class="sxs-lookup"><span data-stu-id="302bc-110">Add a new class toohandle notifications.</span></span> <span data-ttu-id="302bc-111">A Project Explorer nyissa meg a hello **src** > **fő** > **java** csomópontokat, és kattintson a jobb gombbal hello csomag neve csomópont.</span><span class="sxs-lookup"><span data-stu-id="302bc-111">In Project Explorer, open hello **src** > **main** > **java** nodes, and right-click hello package name node.</span></span> <span data-ttu-id="302bc-112">Kattintson a **új**, és kattintson a **Java-osztály**.</span><span class="sxs-lookup"><span data-stu-id="302bc-112">Click **New**, and then click **Java Class**.</span></span>
8. <span data-ttu-id="302bc-113">A **neve**, típus `MyHandler`, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="302bc-113">In **Name**, type `MyHandler`, and then click **OK**.</span></span>

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. <span data-ttu-id="302bc-114">Hello MyHandler fájlban cserélje le a hello osztálydeklaráció:</span><span class="sxs-lookup"><span data-stu-id="302bc-114">In hello MyHandler file, replace hello class declaration with:</span></span>

        public class MyHandler extends NotificationsHandler {
10. <span data-ttu-id="302bc-115">Adja hozzá a következő importálási utasításokat a hello hello `MyHandler` osztály:</span><span class="sxs-lookup"><span data-stu-id="302bc-115">Add hello following import statements for hello `MyHandler` class:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. <span data-ttu-id="302bc-116">Ezután adja hozzá a tag toohello `MyHandler` osztály:</span><span class="sxs-lookup"><span data-stu-id="302bc-116">Next add this member toohello `MyHandler` class:</span></span>

        public static final int NOTIFICATION_ID = 1;
12. <span data-ttu-id="302bc-117">A hello `MyHandler` osztály, adja hozzá a következő kód toooverride hello hello **onRegistered** metódus, amely hello mobilszolgáltatás értesítési központ regisztrálja az eszközt.</span><span class="sxs-lookup"><span data-stu-id="302bc-117">In hello `MyHandler` class, add hello following code toooverride hello **onRegistered** method, which registers your device with hello mobile service notification hub.</span></span>

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
       <span data-ttu-id="302bc-118">}</span><span class="sxs-lookup"><span data-stu-id="302bc-118">}</span></span>
13. <span data-ttu-id="302bc-119">A hello `MyHandler` osztály, adja hozzá a következő kód toooverride hello hello **onReceive** metódus, amely hello értesítési toodisplay okoz érkezik.</span><span class="sxs-lookup"><span data-stu-id="302bc-119">In hello `MyHandler` class, add hello following code toooverride hello **onReceive** method, which causes hello notification toodisplay when it is received.</span></span>

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
       <span data-ttu-id="302bc-120">}</span><span class="sxs-lookup"><span data-stu-id="302bc-120">}</span></span>
14. <span data-ttu-id="302bc-121">Vissza az hello TodoActivity.java fájlban frissítse a hello **onCreate** hello metódusában *ToDoActivity* tooregister hello értesítési kezelő osztály osztályban.</span><span class="sxs-lookup"><span data-stu-id="302bc-121">Back in hello TodoActivity.java file, update hello **onCreate** method of hello *ToDoActivity* class tooregister hello notification handler class.</span></span> <span data-ttu-id="302bc-122">Győződjön meg arról, hogy tooadd Ez a kód után hello *MobileServiceClient* létrejön.</span><span class="sxs-lookup"><span data-stu-id="302bc-122">Make sure tooadd this code after hello *MobileServiceClient* is instantiated.</span></span>

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    <span data-ttu-id="302bc-123">Az alkalmazás már frissített toosupport leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="302bc-123">Your app is now updated toosupport push notifications.</span></span>
