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
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a><span data-ttu-id="8f235-103">Ismerkedés a Notification Hubs szolgáltatással Kindle-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="8f235-103">Get started with Notification Hubs for Kindle apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="8f235-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8f235-104">Overview</span></span>
<span data-ttu-id="8f235-105">Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Kindle-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8f235-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Kindle application.</span></span>
<span data-ttu-id="8f235-106">Létre fog hozni egy üres Kindle-alkalmazást, amely leküldéses értesítéseket fogad az Amazon Device Messaging (ADM) használatával.</span><span class="sxs-lookup"><span data-stu-id="8f235-106">You'll create a blank Kindle app that receives push notifications by using Amazon Device Messaging (ADM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f235-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f235-107">Prerequisites</span></span>
<span data-ttu-id="8f235-108">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="8f235-108">This tutorial requires hello following:</span></span>

* <span data-ttu-id="8f235-109">Android SDK (feltételezzük, hogy Eclipse-et használ) hello beszerezni hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android webhelyéről</a>.</span><span class="sxs-lookup"><span data-stu-id="8f235-109">Get hello Android SDK (we assume that you will use Eclipse) from hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android site</a>.</span></span>
* <span data-ttu-id="8f235-110">Hello kövesse <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">beállítás be a fejlesztési környezet</a> tooset Kindle fejlesztési környezet.</span><span class="sxs-lookup"><span data-stu-id="8f235-110">Follow hello steps in <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Setting Up Your Development Environment</a> tooset up your development environment for Kindle.</span></span>

## <a name="add-a-new-app-toohello-developer-portal"></a><span data-ttu-id="8f235-111">Adja hozzá egy új alkalmazás toohello fejlesztői portálján</span><span class="sxs-lookup"><span data-stu-id="8f235-111">Add a new app toohello developer portal</span></span>
1. <span data-ttu-id="8f235-112">Először hozzon létre egy alkalmazást hello [Amazon fejlesztői portálján].</span><span class="sxs-lookup"><span data-stu-id="8f235-112">First, create an app in hello [Amazon developer portal].</span></span>
   
    ![][0]
2. <span data-ttu-id="8f235-113">Másolás hello **Alkalmazáskulcsot**.</span><span class="sxs-lookup"><span data-stu-id="8f235-113">Copy hello **Application Key**.</span></span>
   
    ![][1]
3. <span data-ttu-id="8f235-114">Hello portálon, kattintson az alkalmazás hello nevére, és kattintson a hello **Device Messaging** fülre.</span><span class="sxs-lookup"><span data-stu-id="8f235-114">In hello portal, click hello name of your app, and then click hello **Device Messaging** tab.</span></span>
   
    ![][2]
4. <span data-ttu-id="8f235-115">Kattintson a **Create a New Security Profile** (Új biztonsági profil létrehozása) elemre, hozzon létre egy új biztonsági profilt (például: **TestAdm security profile**).</span><span class="sxs-lookup"><span data-stu-id="8f235-115">Click **Create a New Security Profile**, and then create a new security profile (for example, **TestAdm security profile**).</span></span> <span data-ttu-id="8f235-116">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f235-116">Then click **Save**.</span></span>
   
    ![][3]
5. <span data-ttu-id="8f235-117">Kattintson a **biztonsági profilok** tooview hello biztonsági profil, most hozott létre.</span><span class="sxs-lookup"><span data-stu-id="8f235-117">Click **Security Profiles** tooview hello security profile that you just created.</span></span> <span data-ttu-id="8f235-118">Másolás hello **ügyfél-azonosító** és **Ügyfélkulcs** értékek későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="8f235-118">Copy hello **Client ID** and **Client Secret** values for later use.</span></span>
   
    ![][4]

## <a name="create-an-api-key"></a><span data-ttu-id="8f235-119">API-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f235-119">Create an API key</span></span>
1. <span data-ttu-id="8f235-120">Nyisson meg egy parancssort rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="8f235-120">Open a command prompt with administrator privileges.</span></span>
2. <span data-ttu-id="8f235-121">Keresse meg a toohello Android SDK mappába.</span><span class="sxs-lookup"><span data-stu-id="8f235-121">Navigate toohello Android SDK folder.</span></span>
3. <span data-ttu-id="8f235-122">Adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="8f235-122">Enter hello following command:</span></span>
   
        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
   
    ![][5]
4. <span data-ttu-id="8f235-123">A hello **keystore** jelszót, írja be **android**.</span><span class="sxs-lookup"><span data-stu-id="8f235-123">For hello **keystore** password, type **android**.</span></span>
5. <span data-ttu-id="8f235-124">Másolás hello **MD5** ujjlenyomat.</span><span class="sxs-lookup"><span data-stu-id="8f235-124">Copy hello **MD5** fingerprint.</span></span>
6. <span data-ttu-id="8f235-125">Vissza a hello fejlesztői portálján hello **Messaging** lapra, majd **Android/Kindle** , és írja be az alkalmazás hello hello csomag nevét (például **com.sample.notificationhubtest**) és hello **MD5** értékét, és kattintson a **API-kulcs létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8f235-125">Back in hello developer portal, on hello **Messaging** tab, click **Android/Kindle** and enter hello name of hello package for your app (for example, **com.sample.notificationhubtest**) and hello **MD5** value, and then click **Generate API Key**.</span></span>

## <a name="add-credentials-toohello-hub"></a><span data-ttu-id="8f235-126">Hitelesítő adatok toohello hub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f235-126">Add credentials toohello hub</span></span>
<span data-ttu-id="8f235-127">Hello portálon, vegye fel a hello ügyfél titkos kulcs és az ügyfél azonosító toohello **konfigurálása** az értesítési központ lapján.</span><span class="sxs-lookup"><span data-stu-id="8f235-127">In hello portal, add hello client secret and client ID toohello **Configure** tab of your notification hub.</span></span>

## <a name="set-up-your-application"></a><span data-ttu-id="8f235-128">Az alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="8f235-128">Set up your application</span></span>
> [!NOTE]
> <span data-ttu-id="8f235-129">Alkalmazás létrehozásakor legalább 17-es API-szintet használjon.</span><span class="sxs-lookup"><span data-stu-id="8f235-129">When you're creating an application, use at least API Level 17.</span></span>
> 
> 

<span data-ttu-id="8f235-130">Hello ADM szalagtárak tooyour Eclipse-projekt hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="8f235-130">Add hello ADM libraries tooyour Eclipse project:</span></span>

1. <span data-ttu-id="8f235-131">tooobtain hello ADM-kódtár [hello SDK letöltése].</span><span class="sxs-lookup"><span data-stu-id="8f235-131">tooobtain hello ADM library, [download hello SDK].</span></span> <span data-ttu-id="8f235-132">Bontsa ki a hello SDK zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="8f235-132">Extract hello SDK zip file.</span></span>
2. <span data-ttu-id="8f235-133">Az Eclipse-ben kattintson a jobb gombbal a projektjére, majd kattintson a **Properties** (Tulajdonságok) elemre.</span><span class="sxs-lookup"><span data-stu-id="8f235-133">In Eclipse, right-click your project, and then click **Properties**.</span></span> <span data-ttu-id="8f235-134">Válassza ki **Java Build elérési** hello maradt, és jelölje ki hello ** szalagtárak ** hello felső fülre.</span><span class="sxs-lookup"><span data-stu-id="8f235-134">Select **Java Build Path** on hello left, and then select hello **Libraries **tab at hello top.</span></span> <span data-ttu-id="8f235-135">Kattintson a **külső Jar hozzáadása**, és jelölje be hello fájl `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` hello könyvtárból, ahová kicsomagolta hello Amazon SDK-t.</span><span class="sxs-lookup"><span data-stu-id="8f235-135">Click **Add External Jar**, and select hello file `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` from hello directory in which you extracted hello Amazon SDK.</span></span>
3. <span data-ttu-id="8f235-136">Töltse le a NotificationHubs Android SDK-t (hivatkozás) hello.</span><span class="sxs-lookup"><span data-stu-id="8f235-136">Download hello NotificationHubs Android SDK (link).</span></span>
4. <span data-ttu-id="8f235-137">Bontsa ki a hello csomagot, és húzza a hello fájl `notification-hubs-sdk.jar` történő hello `libs` mappa az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="8f235-137">Unzip hello package, and then drag hello file `notification-hubs-sdk.jar` into hello `libs` folder in Eclipse.</span></span>

<span data-ttu-id="8f235-138">Az alkalmazás jegyzékének toosupport ADM szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="8f235-138">Edit your app manifest toosupport ADM:</span></span>

1. <span data-ttu-id="8f235-139">Adja hozzá hello Amazon-névteret hello gyökérjegyzék-elemhez:</span><span class="sxs-lookup"><span data-stu-id="8f235-139">Add hello Amazon namespace in hello root manifest element:</span></span>

        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

1. <span data-ttu-id="8f235-140">Engedélyek hozzáadása hello első eleme hello jegyzék elem alatt.</span><span class="sxs-lookup"><span data-stu-id="8f235-140">Add permissions as hello first element under hello manifest element.</span></span> <span data-ttu-id="8f235-141">Helyettesítő **[YOUR PACKAGE NAME]** hello csomaggal, amellyel toocreate az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8f235-141">Substitute **[YOUR PACKAGE NAME]** with hello package that you used toocreate your app.</span></span>
   
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
2. <span data-ttu-id="8f235-142">Helyezze be a hello elem követően hello hello alkalmazáselem első gyermekeként.</span><span class="sxs-lookup"><span data-stu-id="8f235-142">Insert hello following element as hello first child of hello application element.</span></span> <span data-ttu-id="8f235-143">Ne feledje toosubstitute **[YOUR SERVICE NAME]** hello nevű az ADM üzenetkezelőjének létrehozása a következő szakasz hello (többek között a következőket hello csomag), és cserélje le a **[YOUR PACKAGE NAME]** a hello az alkalmazást létrehozó csomag neve.</span><span class="sxs-lookup"><span data-stu-id="8f235-143">Remember toosubstitute **[YOUR SERVICE NAME]** with hello name of your ADM message handler that you create in hello next section (including hello package), and replace **[YOUR PACKAGE NAME]** with hello package name with which you created your app.</span></span>
   
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

## <a name="create-your-adm-message-handler"></a><span data-ttu-id="8f235-144">Az ADM üzenetkezelőjének létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f235-144">Create your ADM message handler</span></span>
1. <span data-ttu-id="8f235-145">Hozzon létre egy új osztályt, amely örökli `com.amazon.device.messaging.ADMMessageHandlerBase` és adjon neki nevet `MyADMMessageHandler`, ahogy az ábra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8f235-145">Create a new class that inherits from `com.amazon.device.messaging.ADMMessageHandlerBase` and name it `MyADMMessageHandler`, as shown in hello following figure:</span></span>
   
    ![][6]
2. <span data-ttu-id="8f235-146">Adja hozzá a következő hello `import` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="8f235-146">Add hello following `import` statements:</span></span>
   
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub
3. <span data-ttu-id="8f235-147">Adja hozzá a következő kódot a létrehozott osztályhoz hello hello.</span><span class="sxs-lookup"><span data-stu-id="8f235-147">Add hello following code in hello class that you created.</span></span> <span data-ttu-id="8f235-148">Ne feledje toosubstitute hello központ nevét és a kapcsolati karakterláncot (Figyelés):</span><span class="sxs-lookup"><span data-stu-id="8f235-148">Remember toosubstitute hello hub name and connection string (listen):</span></span>
   
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
4. <span data-ttu-id="8f235-149">Adja hozzá a következő kód toohello hello `OnMessage()` módszert:</span><span class="sxs-lookup"><span data-stu-id="8f235-149">Add hello following code toohello `OnMessage()` method:</span></span>
   
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
5. <span data-ttu-id="8f235-150">Adja hozzá a következő kód toohello hello `OnRegistered` módszert:</span><span class="sxs-lookup"><span data-stu-id="8f235-150">Add hello following code toohello `OnRegistered` method:</span></span>
   
            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }
6. <span data-ttu-id="8f235-151">Adja hozzá a következő kód toohello hello `OnUnregistered` módszert:</span><span class="sxs-lookup"><span data-stu-id="8f235-151">Add hello following code toohello `OnUnregistered` method:</span></span>
   
         try {
             getNotificationHub(getApplicationContext()).unregister();
         } catch (Exception e) {
             Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
         }
7. <span data-ttu-id="8f235-152">A hello `MainActivity` módszer, adja hozzá a következő importálási utasítást hello:</span><span class="sxs-lookup"><span data-stu-id="8f235-152">In hello `MainActivity` method, add hello following import statement:</span></span>
   
        import com.amazon.device.messaging.ADM;
8. <span data-ttu-id="8f235-153">Adja hozzá a következő kód hello hello végén hello `OnCreate` módszert:</span><span class="sxs-lookup"><span data-stu-id="8f235-153">Add hello following code at hello end of hello `OnCreate` method:</span></span>
   
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

## <a name="add-your-api-key-tooyour-app"></a><span data-ttu-id="8f235-154">Az API-kulcs tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8f235-154">Add your API key tooyour app</span></span>
1. <span data-ttu-id="8f235-155">Az eclipse-ben hozzon létre egy új fájlt **api_key.txt** a hello a projekt könyvtáreszközeiben.</span><span class="sxs-lookup"><span data-stu-id="8f235-155">In Eclipse, create a new file named **api_key.txt** in hello directory assets of your project.</span></span>
2. <span data-ttu-id="8f235-156">Nyissa meg a hello fájlt, és másolja hello API-kulcs hello Amazon fejlesztői portálon létrehozott.</span><span class="sxs-lookup"><span data-stu-id="8f235-156">Open hello file and copy hello API key that you generated in hello Amazon developer portal.</span></span>

## <a name="run-hello-app"></a><span data-ttu-id="8f235-157">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="8f235-157">Run hello app</span></span>
1. <span data-ttu-id="8f235-158">Indítsa el a hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="8f235-158">Start hello emulator.</span></span>
2. <span data-ttu-id="8f235-159">Hello emulátorban, csúsztassa az ujját hello fentről, és kattintson a **beállítások**, és kattintson a **fiókomat** és egy érvényes Amazon-fiókot regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="8f235-159">In hello emulator, swipe from hello top and click **Settings**, and then click **My account** and register with a valid Amazon account.</span></span>
3. <span data-ttu-id="8f235-160">Az eclipse-ben futtassa a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8f235-160">In Eclipse, run hello app.</span></span>

> [!NOTE]
> <span data-ttu-id="8f235-161">Ha a probléma akkor fordul elő, ellenőrizze a hello idő hello emulátor (vagy egy eszköz).</span><span class="sxs-lookup"><span data-stu-id="8f235-161">If a problem occurs, check hello time of hello emulator (or device).</span></span> <span data-ttu-id="8f235-162">hello idő értékének pontosnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8f235-162">hello time value must be accurate.</span></span> <span data-ttu-id="8f235-163">hello Kindle-emulátor toochange hello ideje, futtathatja a következő parancsot az Android SDK platformeszközök könyvtárából hello:</span><span class="sxs-lookup"><span data-stu-id="8f235-163">toochange hello time of hello Kindle emulator, you can run hello following command from your Android SDK platform-tools directory:</span></span>
> 
> 

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a><span data-ttu-id="8f235-164">Üzenet küldése</span><span class="sxs-lookup"><span data-stu-id="8f235-164">Send a message</span></span>
<span data-ttu-id="8f235-165">toosend üzenet .NET használatával:</span><span class="sxs-lookup"><span data-stu-id="8f235-165">toosend a message by using .NET:</span></span>

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
