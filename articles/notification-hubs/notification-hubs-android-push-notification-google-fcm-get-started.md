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
# <a name="sending-push-notifications-tooandroid-with-azure-notification-hubs"></a><span data-ttu-id="df336-104">Az Azure Notification Hubs leküldéses értesítések tooAndroid küldése</span><span class="sxs-lookup"><span data-stu-id="df336-104">Sending push notifications tooAndroid with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="df336-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="df336-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="df336-106">Ez a témakör a leküldéses értesítések Google Firebase Cloud Messaging (FCM) használatával történő küldését mutatja be.</span><span class="sxs-lookup"><span data-stu-id="df336-106">This topic demonstrates push notifications with Google Firebase Cloud Messaging (FCM).</span></span> <span data-ttu-id="df336-107">Ha a Google Cloud Messaging (GCM) továbbra is használ, tekintse meg [küldő leküldéses értesítések tooAndroid az Azure Notification Hubs és a GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="df336-107">If you are still using Google Cloud Messaging (GCM), see [Sending push notifications tooAndroid with Azure Notification Hubs and GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).</span></span>
> 
> 

<span data-ttu-id="df336-108">Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs és Firebase Cloud Messaging toosend leküldéses értesítések tooan Android-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="df336-108">This tutorial shows you how toouse Azure Notification Hubs and Firebase Cloud Messaging toosend push notifications tooan Android application.</span></span>
<span data-ttu-id="df336-109">Létre fog hozni egy üres Android-alkalmazást, amely leküldéses értesítéseket fogad a Firebase Cloud Messaging (FCM) használatával.</span><span class="sxs-lookup"><span data-stu-id="df336-109">You'll create a blank Android app that receives push notifications by using Firebase Cloud Messaging (FCM).</span></span>

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="df336-110">az oktatóanyag kódjának befejeződött hello letölthető a GitHub [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).</span><span class="sxs-lookup"><span data-stu-id="df336-110">hello completed code for this tutorial can be downloaded from GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df336-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="df336-111">Prerequisites</span></span>
> [!IMPORTANT]
> <span data-ttu-id="df336-112">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="df336-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="df336-113">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="df336-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="df336-114">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="df336-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span></span>
> 
> 

* <span data-ttu-id="df336-115">Továbbá tooan aktív Azure-fiók említettük, ez az oktatóanyag hello legújabb verziója szükséges [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span><span class="sxs-lookup"><span data-stu-id="df336-115">In addition tooan active Azure account mentioned above, this tutorial requires hello latest version of [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span></span>
* <span data-ttu-id="df336-116">Android 2.3 vagy újabb a Firebase Cloud Messaging esetében.</span><span class="sxs-lookup"><span data-stu-id="df336-116">Android 2.3 or higher for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="df336-117">A Google-tárház 27-es vagy újabb verziója a Firebase Cloud Messaging esetében.</span><span class="sxs-lookup"><span data-stu-id="df336-117">Google Repository revision 27 or higher is required for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="df336-118">A Google Play-szolgáltatások 9.0.2-es vagy újabb verziója a Firebase Cloud Messaging esetében.</span><span class="sxs-lookup"><span data-stu-id="df336-118">Google Play Services 9.0.2 or higher for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="df336-119">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Android-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="df336-119">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Android apps.</span></span>

## <a name="create-a-new-android-studio-project"></a><span data-ttu-id="df336-120">Új Android Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="df336-120">Create a new Android Studio Project</span></span>
1. <span data-ttu-id="df336-121">Az Android Studióban indítson el egy új Android Studio-projektet.</span><span class="sxs-lookup"><span data-stu-id="df336-121">In Android Studio, start a new Android Studio project.</span></span>
   
       ![Android Studio - new project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. <span data-ttu-id="df336-122">Válassza ki a hello **telefon és táblagép** tényező és hello **minimális SDK** , amelyet az toosupport.</span><span class="sxs-lookup"><span data-stu-id="df336-122">Choose hello **Phone and Tablet** form factor and hello **Minimum SDK** that you want toosupport.</span></span> <span data-ttu-id="df336-123">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="df336-123">Then click **Next**.</span></span>
   
       ![Android Studio - project creation workflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. <span data-ttu-id="df336-124">Válasszon **üres tevékenység** hello fő tevékenység, kattintson a **következő**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="df336-124">Choose **Empty Activity** for hello main activity, click **Next**, and then click **Finish**.</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="df336-125">A Firebase Cloud Messaginget támogató projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="df336-125">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a><span data-ttu-id="df336-126">Új értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="df336-126">Configure a new notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="df336-127">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="df336-127">&emsp;&emsp;6.</span></span> <span data-ttu-id="df336-128">A hello **beállítások** az értesítési központ, jelölje be a panelt **értesítési szolgáltatások** , majd **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="df336-128">In hello **Settings** blade of your notification hub, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="df336-129">Adja meg a korábban kimásolt a hello hello FCM kiszolgálókulcs [Firebase konzol](https://firebase.google.com/console/) kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="df336-129">Enter hello FCM server key you copied earlier from hello [Firebase console](https://firebase.google.com/console/) and click **Save**.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

<span data-ttu-id="df336-131">Az értesítési központ most Firebase felhő Messagin a konfigurált toowork, és van hello kapcsolati karakterláncok tooboth regisztrálja az alkalmazást tooreceive, valamint leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="df336-131">Your notification hub is now configured toowork with Firebase Cloud Messagin, and you have hello connection strings tooboth register your app tooreceive and send push notifications.</span></span>

## <span data-ttu-id="df336-132"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="df336-132"><a id="connecting-app"></a>Connect your app toohello notification hub</span></span>
### <a name="add-google-play-services-toohello-project"></a><span data-ttu-id="df336-133">Google Play services toohello projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="df336-133">Add Google Play services toohello project</span></span>
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a><span data-ttu-id="df336-134">Azure Notification Hubs-kódtárak felvétele</span><span class="sxs-lookup"><span data-stu-id="df336-134">Adding Azure Notification Hubs libraries</span></span>
1. <span data-ttu-id="df336-135">A hello `Build.Gradle` hello fájlt **app**, adja hozzá a következő sorokat hello hello **függőségek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="df336-135">In hello `Build.Gradle` file for hello **app**, add hello following lines in hello **dependencies** section.</span></span>
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. <span data-ttu-id="df336-136">Adja hozzá a következő tárházat után hello hello **függőségek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="df336-136">Add hello following repository after hello **dependencies** section.</span></span>
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-hello-androidmanifestxml"></a><span data-ttu-id="df336-137">Hello AndroidManifest.xml frissítése.</span><span class="sxs-lookup"><span data-stu-id="df336-137">Updating hello AndroidManifest.xml.</span></span>
1. <span data-ttu-id="df336-138">toosupport FCM, létre kell hoznunk egy Példányazonosító-figyelő szolgáltatást a kódban, amely túl[regisztrációs jogkivonatok lekérésére](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) használatával [Google FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId).</span><span class="sxs-lookup"><span data-stu-id="df336-138">toosupport FCM, we must implement a Instance ID listener service in our code which is used too[obtain registration tokens](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) using [Google's FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId).</span></span> <span data-ttu-id="df336-139">Az oktatóanyag azt hello osztály nevet `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="df336-139">In this tutorial we will name hello class `MyInstanceIDService`.</span></span> 
   
    <span data-ttu-id="df336-140">Adja hozzá a következő szolgáltatás definíciós toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="df336-140">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> 
   
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
2. <span data-ttu-id="df336-141">Miután a FCM regisztrációs jogkivonat a hello FirebaseInstanceId API, ezzel túl[hello Azure Notification Hub regisztrálása](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="df336-141">Once we have received our FCM registration token from hello FirebaseInstanceId API, we will use it too[register with hello Azure Notification Hub](notification-hubs-push-notification-registration-management.md).</span></span> <span data-ttu-id="df336-142">Ez a regisztráció támogatjuk hello háttér használatával egy `IntentService` nevű `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="df336-142">We will support this registration in hello background using an `IntentService` named `RegistrationIntentService`.</span></span> <span data-ttu-id="df336-143">Ez a szolgáltatás az FCM regisztrációs jogkivonat frissítését is el fogja végezni.</span><span class="sxs-lookup"><span data-stu-id="df336-143">This service will also be responsible for refreshing our FCM registration token.</span></span>
   
    <span data-ttu-id="df336-144">Adja hozzá a következő szolgáltatás definíciós toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="df336-144">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> 
   
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
3. <span data-ttu-id="df336-145">A fogadó tooreceive értesítéseket is meghatározunk.</span><span class="sxs-lookup"><span data-stu-id="df336-145">We will also define a receiver tooreceive notifications.</span></span> <span data-ttu-id="df336-146">Adja hozzá a következő fogadó definition toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="df336-146">Add hello following receiver definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="df336-147">Cserélje le a hello `<your package>` helyőrzőt hello hello hello tetején látható tényleges csomagnévre `AndroidManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="df336-147">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="df336-148">Adja hozzá a következő szükséges FCM hello kapcsolatos alábbi hello engedélyek `</application>` címke.</span><span class="sxs-lookup"><span data-stu-id="df336-148">Add hello following necessary FCM related permissions below hello  `</application>` tag.</span></span> <span data-ttu-id="df336-149">Győződjön meg arról, hogy tooreplace `<your package>` hello hello tetején látható hello csomag nevű `AndroidManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="df336-149">Make sure tooreplace `<your package>` with hello package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
    <span data-ttu-id="df336-150">Ezekről az engedélyekről további információkért lásd: [GCM-ügyfélalkalmazás beállítása androidhoz](https://developers.google.com/cloud-messaging/android/client#manifest) és [telepítse át a GCM-ügyfélalkalmazás az Android tooFirebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).</span><span class="sxs-lookup"><span data-stu-id="df336-150">For more information on these permissions, see [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest) and [Migrate a GCM Client App for Android tooFirebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

### <a name="adding-code"></a><span data-ttu-id="df336-151">Kód felvétele</span><span class="sxs-lookup"><span data-stu-id="df336-151">Adding code</span></span>
1. <span data-ttu-id="df336-152">A projekt nézetben hello, bontsa ki a **app** > **src** > **fő** > **java**.</span><span class="sxs-lookup"><span data-stu-id="df336-152">In hello Project View, expand **app** > **src** > **main** > **java**.</span></span> <span data-ttu-id="df336-153">Kattintson a jobb gombbal a **java** területen látható csomagmappára, kattintson a **New** (Új), majd a **Java Class** (Java-osztály) elemre.</span><span class="sxs-lookup"><span data-stu-id="df336-153">Right-click your package folder under **java**, click **New**, and then click **Java Class**.</span></span> <span data-ttu-id="df336-154">Adjon hozzá egy új, `NotificationSettings` nevű osztályt.</span><span class="sxs-lookup"><span data-stu-id="df336-154">Add a new class named `NotificationSettings`.</span></span> 
   
    ![Android Studio – új Java-osztály](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    <span data-ttu-id="df336-156">Ellenőrizze, hogy tooupdate hello ezek hello hello kódját a következő három helyőrzőt `NotificationSettings` osztály:</span><span class="sxs-lookup"><span data-stu-id="df336-156">Make sure tooupdate hello these three placeholders in hello following code for hello `NotificationSettings` class:</span></span>
   
   * <span data-ttu-id="df336-157">**SenderId**: hello korábban beolvasott Küldőazonosító hello **Cloud Messaging** szereplő hello projekt beállítások lapon [Firebase konzol](https://firebase.google.com/console/).</span><span class="sxs-lookup"><span data-stu-id="df336-157">**SenderId**: hello Sender Id you obtained earlier in hello **Cloud Messaging** tab of your project settings in hello [Firebase console](https://firebase.google.com/console/).</span></span>
   * <span data-ttu-id="df336-158">**HubListenConnectionString**: hello **DefaultListenAccessSignature** kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="df336-158">**HubListenConnectionString**: hello **DefaultListenAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="df336-159">Kapcsolati karakterlánc másolhatja kattintva **hozzáférési házirendek** a hello **beállítások** hello központ paneljén [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="df336-159">You can copy that connection string by clicking **Access Policies** on hello **Settings** blade of your hub on hello [Azure Portal].</span></span>
   * <span data-ttu-id="df336-160">**HubName**: hello név használata hello hello központ paneljén megjelenő értesítési központ [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="df336-160">**HubName**: Use hello name of your notification hub that appears in hello hub blade in hello [Azure Portal].</span></span>
     
     <span data-ttu-id="df336-161">`NotificationSettings` kód:</span><span class="sxs-lookup"><span data-stu-id="df336-161">`NotificationSettings` code:</span></span>
     
       <span data-ttu-id="df336-162">public class NotificationSettings {</span><span class="sxs-lookup"><span data-stu-id="df336-162">public class NotificationSettings {</span></span>
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       <span data-ttu-id="df336-163">}</span><span class="sxs-lookup"><span data-stu-id="df336-163">}</span></span>
2. <span data-ttu-id="df336-164">Használatával a hello fent leírt lépésekkel, vegyen fel egy másik új osztályt `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="df336-164">Using hello steps above, add another new class named `MyInstanceIDService`.</span></span> <span data-ttu-id="df336-165">Ez lesz az általunk megvalósított Példányazonosító figyelőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="df336-165">This will be our Instance ID listener service implementation.</span></span>
   
    <span data-ttu-id="df336-166">Ez az osztály kódját hello szolgáltatás fel fogja hívni a `IntentService` túl[frissítési hello FCM jogkivonat](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) hello háttérben.</span><span class="sxs-lookup"><span data-stu-id="df336-166">hello code for this class will call our `IntentService` too[refresh hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) in hello background.</span></span>
   
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


1. <span data-ttu-id="df336-167">Adja hozzá egy másik új osztály tooyour nevű projekt, `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="df336-167">Add another new class tooyour project named, `RegistrationIntentService`.</span></span> <span data-ttu-id="df336-168">Ez lesz a hello megvalósítását a `IntentService` , amely elvégzi [frissítés hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) és [hello értesítési központ regisztrálása](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="df336-168">This will be hello implementation for our `IntentService` that will handle [refreshing hello FCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) and [registering with hello notification hub](notification-hubs-push-notification-registration-management.md).</span></span>
   
    <span data-ttu-id="df336-169">Ez az osztály kódját a következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="df336-169">Use hello following code for this class.</span></span>
   
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
2. <span data-ttu-id="df336-170">Az a `MainActivity` osztály, adja hozzá a következő hello `import` fent hello utasítások osztály deklarációjában.</span><span class="sxs-lookup"><span data-stu-id="df336-170">In your `MainActivity` class, add hello following `import` statements above hello class declaration.</span></span>
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. <span data-ttu-id="df336-171">Adja hozzá az alábbi privát tagokat hello osztály hello tetején hello.</span><span class="sxs-lookup"><span data-stu-id="df336-171">Add hello following private members at hello top of hello class.</span></span> <span data-ttu-id="df336-172">Ezek használjuk [Google Play-szolgáltatások hello elérhetőségének ellenőrzése a Google által javasolt módon](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span><span class="sxs-lookup"><span data-stu-id="df336-172">We will use these [check hello availability of Google Play Services as recommended by Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span></span>
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. <span data-ttu-id="df336-173">Az a `MainActivity` osztály, adja hozzá a következő metódus toohello Google Play-szolgáltatások rendelkezésre állásának hello.</span><span class="sxs-lookup"><span data-stu-id="df336-173">In your `MainActivity` class, add hello following method toohello availability of Google Play Services.</span></span> 
   
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
5. <span data-ttu-id="df336-174">Az a `MainActivity` osztály, adja hozzá a következő kódot, amellyel a Google Play-szolgáltatások hívása előtt hello a `IntentService` tooget a FCM regisztrációs jogkivonat és regisztrálása az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="df336-174">In your `MainActivity` class, add hello following code that will check for Google Play Services before calling your `IntentService` tooget your FCM registration token and register with your notification hub.</span></span>
   
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService tooregister this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. <span data-ttu-id="df336-175">A hello `OnCreate` hello metódusában `MainActivity` osztály, adja hozzá a hello tevékenység létrehozásakor a következő kód toostart hello regisztrációs folyamat során.</span><span class="sxs-lookup"><span data-stu-id="df336-175">In hello `OnCreate` method of hello `MainActivity` class, add hello following code toostart hello registration process when activity is created.</span></span>
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. <span data-ttu-id="df336-176">Adja hozzá a további módszereket toohello `MainActivity` tooverify alkalmazások állapotának és a jelentés az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="df336-176">Add these additional methods toohello `MainActivity` tooverify app state and report status in your app.</span></span>
   
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
8. <span data-ttu-id="df336-177">Hello `ToastNotify` metódusnak hello *"Hello World"* `TextView` tooreport állapot és az értesítések folyamatos jelentéséhez az hello app szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="df336-177">hello `ToastNotify` method uses hello *"Hello World"* `TextView` control tooreport status and notifications persistently in hello app.</span></span> <span data-ttu-id="df336-178">Az activity_main.xml elrendezésben vegye fel az adott vezérlő azonosítója a következő hello.</span><span class="sxs-lookup"><span data-stu-id="df336-178">In your activity_main.xml layout, add hello following id for that control.</span></span>
   
       android:id="@+id/text_hello"
9. <span data-ttu-id="df336-179">A Tovább gombra a fogadó hello AndroidManifest.xml a meghatározott a adunk hozzá egy alosztályt.</span><span class="sxs-lookup"><span data-stu-id="df336-179">Next we will add a subclass for our receiver we defined in hello AndroidManifest.xml.</span></span> <span data-ttu-id="df336-180">Adja hozzá egy másik új osztály tooyour nevű projekt `MyHandler`.</span><span class="sxs-lookup"><span data-stu-id="df336-180">Add another new class tooyour project named `MyHandler`.</span></span>
10. <span data-ttu-id="df336-181">Adja hozzá a következő importálási utasításokat a felső hello hello `MyHandler.java`:</span><span class="sxs-lookup"><span data-stu-id="df336-181">Add hello following import statements at hello top of `MyHandler.java`:</span></span>
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. <span data-ttu-id="df336-182">Adja hozzá a következő kódot a hello hello `MyHandler` osztály, így a alosztálya `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span><span class="sxs-lookup"><span data-stu-id="df336-182">Add hello following code for hello `MyHandler` class making it a subclass of `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span></span>
    
    <span data-ttu-id="df336-183">Ez a kód felülírja hello `OnReceive` metódust, így hello kezelő jelentést küld a kapott értesítésekről.</span><span class="sxs-lookup"><span data-stu-id="df336-183">This code overrides hello `OnReceive` method, so hello handler will report notifications that are received.</span></span> <span data-ttu-id="df336-184">hello értesítéskezelőnek is elküldi a hello leküldéses értesítési toohello Android értesítéskezelőjén hello segítségével `sendNotification()` metódust.</span><span class="sxs-lookup"><span data-stu-id="df336-184">hello handler also sends hello push notification toohello Android notification manager by using hello `sendNotification()` method.</span></span> <span data-ttu-id="df336-185">Hello `sendNotification()` metódust akkor kell végrehajtani, amikor hello alkalmazás nem fut, és értesítés érkezik.</span><span class="sxs-lookup"><span data-stu-id="df336-185">hello `sendNotification()` method should be executed when hello app is not running and a notification is received.</span></span>
    
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
12. <span data-ttu-id="df336-186">Az Android Studióban hello menüsávjában kattintson **Build** > **Rebuild Project** toomake meg arról, hogy a kód a jelen-e hibák.</span><span class="sxs-lookup"><span data-stu-id="df336-186">In Android Studio on hello menu bar, click **Build** > **Rebuild Project** toomake sure that no errors are present in your code.</span></span>
13. <span data-ttu-id="df336-187">Hello alkalmazás futtatható az eszközön, és ellenőrizze, hogy sikeresen hello értesítési központban regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="df336-187">Run hello app on your device and verify it registers successfully with hello notification hub.</span></span> 
    
    > [!NOTE]
    > <span data-ttu-id="df336-188">Regisztráció sikertelen hello kezdeti indítási amíg hello `onTokenRefresh()` módszer azonosító service-példány neve.</span><span class="sxs-lookup"><span data-stu-id="df336-188">Registration may fail on hello initial launch until hello `onTokenRefresh()` method of instance Id service is called.</span></span> <span data-ttu-id="df336-189">hello frissítési kell kezdeményezhet sikeres regisztráció hello értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="df336-189">hello refresh should intiate a successful registration with hello notification hub.</span></span>
    > 
    > 

## <a name="sending-push-notifications"></a><span data-ttu-id="df336-190">Leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="df336-190">Sending push notifications</span></span>
<span data-ttu-id="df336-191">Leküldéses értesítések fogadásának az alkalmazásban keresztül hello elküldésével tesztelheti [Azure Portal] -hello keressen **hibaelhárítás** hello központ paneljén szakasz alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="df336-191">You can test receiving push notifications in your app by sending them via hello [Azure Portal] - look for hello **Troubleshooting** Section in hello hub blade, as shown below.</span></span>

![Azure Notification Hubs – küldés tesztelése](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-hello-app"></a><span data-ttu-id="df336-193">(Választható) Leküldéses értesítések küldéséhez közvetlenül hello alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="df336-193">(Optional) Send push notifications directly from hello app</span></span>
> [!IMPORTANT]
> <span data-ttu-id="df336-194">Ebben a példában a hello ügyfélalkalmazás értesítések küldésére szolgáló jelleggel valósul meg.</span><span class="sxs-lookup"><span data-stu-id="df336-194">This example of sending notifications from hello client app is provided for learning purposes only.</span></span> <span data-ttu-id="df336-195">Mivel ezt a beállítást hello `DefaultFullSharedAccessSignature` toobe hello ügyfélalkalmazás megtalálható, azt mutatja meg az értesítési központ toohello kockázata, hogy a felhasználó szerezhetnek hozzáférés nem engedélyezett toosend értesítések tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="df336-195">Since this will require hello `DefaultFullSharedAccessSignature` toobe present on hello client app, it exposes your notification hub toohello risk that a user may gain access toosend unauthorized notifications tooyour clients.</span></span>
> 
> 

<span data-ttu-id="df336-196">Az értesítések elküldése általában háttérkiszolgáló használatával történik.</span><span class="sxs-lookup"><span data-stu-id="df336-196">Normally, you would send notifications using a backend server.</span></span> <span data-ttu-id="df336-197">Bizonyos esetekben előfordulhat, hogy toobe képes toosend leküldéses értesítések közvetlenül ügyfélalkalmazástól hello keresi.</span><span class="sxs-lookup"><span data-stu-id="df336-197">For some cases, you might want toobe able toosend push notifications directly from hello client application.</span></span> <span data-ttu-id="df336-198">Ez a szakasz ismerteti, hogyan toosend értesítések hello ügyfélről hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="df336-198">This section explains how toosend notifications from hello client using hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span></span>

1. <span data-ttu-id="df336-199">Az Android Studio projektnézetében bontsa ki a következőt: **App** > **src** > **main** > **res** > **layout**.</span><span class="sxs-lookup"><span data-stu-id="df336-199">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **layout**.</span></span> <span data-ttu-id="df336-200">Nyissa meg hello `activity_main.xml` elrendezés fájlra, majd kattintson a hello **szöveg** tooupdate hello szöveg hello fájl tartalma fülre.</span><span class="sxs-lookup"><span data-stu-id="df336-200">Open hello `activity_main.xml` layout file and click hello **Text** tab tooupdate hello text contents of hello file.</span></span> <span data-ttu-id="df336-201">Frissítse hello kódot, amely hozzáadja az új `Button` és `EditText` küldési vezérlők leküldéses értesítési üzenetek toohello értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="df336-201">Update it with hello code below, which adds new `Button` and `EditText` controls for sending push notification messages toohello notification hub.</span></span> <span data-ttu-id="df336-202">Mielőtt csak hello lap alján, adja hozzá ezt a kódot `</RelativeLayout>`.</span><span class="sxs-lookup"><span data-stu-id="df336-202">Add this code at hello bottom, just before `</RelativeLayout>`.</span></span>
   
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
2. <span data-ttu-id="df336-203">Az Android Studio projektnézetében bontsa ki a következőt: **App** > **src** > **main** > **res** > **values**.</span><span class="sxs-lookup"><span data-stu-id="df336-203">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **values**.</span></span> <span data-ttu-id="df336-204">Nyissa meg hello `strings.xml` fájlt, és új hello által hivatkozott hello karakterlánc-értékek `Button` és `EditText` szabályozza.</span><span class="sxs-lookup"><span data-stu-id="df336-204">Open hello `strings.xml` file and add hello string values that are referenced by hello new `Button` and `EditText` controls.</span></span> <span data-ttu-id="df336-205">Adja hozzá ezek hello fájl hello alján csak előtt `</resources>`.</span><span class="sxs-lookup"><span data-stu-id="df336-205">Add these at hello bottom of hello file, just before `</resources>`.</span></span>
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. <span data-ttu-id="df336-206">Az a `NotificationSetting.java` fájlt, adja hozzá a következő beállítás toohello hello `NotificationSettings` osztály.</span><span class="sxs-lookup"><span data-stu-id="df336-206">In your `NotificationSetting.java` file, add hello following setting toohello `NotificationSettings` class.</span></span>
   
    <span data-ttu-id="df336-207">Frissítés `HubFullAccess` a hello **DefaultFullSharedAccessSignature** kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="df336-207">Update `HubFullAccess` with hello **DefaultFullSharedAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="df336-208">Ez a kapcsolati karakterlánc átmásolhatók a hello [Azure Portal] kattintva **hozzáférési házirendek** a hello **beállítások** az értesítési központ paneljén.</span><span class="sxs-lookup"><span data-stu-id="df336-208">This connection string can be copied from hello [Azure Portal] by clicking **Access Policies** on hello **Settings** blade for your notification hub.</span></span>
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";
4. <span data-ttu-id="df336-209">Az a `MainActivity.java` fájlt, adja hozzá a következő hello `import` fent hello utasítások `MainActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="df336-209">In your `MainActivity.java` file, add hello following `import` statements above hello `MainActivity` class.</span></span>
   
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
5. <span data-ttu-id="df336-210">Az a `MainActivity.java` fájlt, adja hozzá a következő hello hello tetején a tagok hello `MainActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="df336-210">In your `MainActivity.java` file, add hello following members at hello top of hello `MainActivity` class.</span></span>    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. <span data-ttu-id="df336-211">Létre kell hoznia egy szoftverfrissítési hozzáférésű Jogosultságkód (SaS-) token tooauthenticate egy POST kérést toosend üzenetek tooyour értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="df336-211">You must create a Software Access Signature (SaS) token tooauthenticate a POST request toosend messages tooyour notification hub.</span></span> <span data-ttu-id="df336-212">Ehhez hello hello kapcsolati karakterlánc legfontosabb adatainak elemzése és SaS-jogkivonatot, majd hozza létre hello, ahogyan az hello [általánosan használt fogalmakat ismertető](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API-referenciában.</span><span class="sxs-lookup"><span data-stu-id="df336-212">This is done by parsing hello key data from hello connection string and then creating hello SaS token, as mentioned in hello [Common Concepts](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API reference.</span></span> <span data-ttu-id="df336-213">a következő kód hello egy megvalósítási példát szemléltet.</span><span class="sxs-lookup"><span data-stu-id="df336-213">hello following code is an example implementation.</span></span>
   
    <span data-ttu-id="df336-214">A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály tooparse a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="df336-214">In `MainActivity.java`, add hello following method toohello `MainActivity` class tooparse your connection string.</span></span>
   
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
7. <span data-ttu-id="df336-215">A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály toocreate egy SaS hitelesítési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="df336-215">In `MainActivity.java`, add hello following method toohello `MainActivity` class toocreate a SaS authentication token.</span></span>
   
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
8. <span data-ttu-id="df336-216">A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály toohandle hello **értesítés küldése** gomb és üzenet toohello központ használatával hello beépített REST API hello leküldéses értesítést küldeni.</span><span class="sxs-lookup"><span data-stu-id="df336-216">In `MainActivity.java`, add hello following method toohello `MainActivity` class toohandle hello **Send Notification** button click and send hello push notification message toohello hub by using hello built-in REST API.</span></span>
   
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

## <a name="testing-your-app"></a><span data-ttu-id="df336-217">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="df336-217">Testing your app</span></span>
#### <a name="push-notifications-in-hello-emulator"></a><span data-ttu-id="df336-218">Leküldéses értesítések emulátorban hello</span><span class="sxs-lookup"><span data-stu-id="df336-218">Push notifications in hello emulator</span></span>
<span data-ttu-id="df336-219">Ha azt szeretné, hogy tootest leküldéses értesítések emulátorban, győződjön meg arról, hogy az emulátor rendszerképe támogatja-e a hello Google API-szintet az alkalmazás számára is választott.</span><span class="sxs-lookup"><span data-stu-id="df336-219">If you want tootest push notifications inside an emulator, make sure that your emulator image supports hello Google API level that you chose for your app.</span></span> <span data-ttu-id="df336-220">Ha a lemezkép nem támogatja a natív Google API-k, akkor rendszer végül hello **szolgáltatás\_nem\_elérhető** kivétel.</span><span class="sxs-lookup"><span data-stu-id="df336-220">If your image doesn't support native Google APIs, you will end up with hello **SERVICE\_NOT\_AVAILABLE** exception.</span></span>

<span data-ttu-id="df336-221">Ezenkívül toohello fenti, győződjön meg arról, hogy hozzáadta a Google-fiók az emulátor futtató tooyour **beállítások** > **fiókok**.</span><span class="sxs-lookup"><span data-stu-id="df336-221">In addition toohello above, ensure that you have added your Google account tooyour running emulator under **Settings** > **Accounts**.</span></span> <span data-ttu-id="df336-222">Ellenkező esetben a GCM-mel kísérletek tooregister hello eredményezhet **hitelesítési\_sikertelen** kivétel.</span><span class="sxs-lookup"><span data-stu-id="df336-222">Otherwise, your attempts tooregister with GCM may result in hello **AUTHENTICATION\_FAILED** exception.</span></span>

#### <a name="running-hello-application"></a><span data-ttu-id="df336-223">Hello alkalmazást futtat</span><span class="sxs-lookup"><span data-stu-id="df336-223">Running hello application</span></span>
1. <span data-ttu-id="df336-224">Hello alkalmazás futtatását, és figyelje meg, hogy sikeres regisztráció jelentett-e hello regisztrációs Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="df336-224">Run hello app and notice that hello registration ID is reported for a successful registration.</span></span>
   
       ![Testing on Android - Channel registration](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. <span data-ttu-id="df336-225">Adjon meg egy értesítési üzenetet toobe tooall Android-eszközök hello központon regisztrált küldött.</span><span class="sxs-lookup"><span data-stu-id="df336-225">Enter a notification message toobe sent tooall Android devices that have registered with hello hub.</span></span>
   
       ![Testing on Android - sending a message](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. <span data-ttu-id="df336-226">Nyomja le a **Send Notification** (Értesítés küldése) gombot.</span><span class="sxs-lookup"><span data-stu-id="df336-226">Press **Send Notification**.</span></span> <span data-ttu-id="df336-227">Hello app futó összes eszközön megjelenik egy `AlertDialog` példány hello leküldéses értesítési üzenettel.</span><span class="sxs-lookup"><span data-stu-id="df336-227">Any devices that have hello app running will show an `AlertDialog` instance with hello push notification message.</span></span> <span data-ttu-id="df336-228">A leküldéses értesítések, amelyek nem rendelkeznek hello alkalmazás fut, de korábban már regisztrált eszközök az Android Értesítéskezelőjén hello egy értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="df336-228">Devices that don't have hello app running but were previously registered for push notifications will receive a notification in hello Android Notification Manager.</span></span> <span data-ttu-id="df336-229">Azok az hello bal felső sarokból lefelé pöccintve tekinthetők meg.</span><span class="sxs-lookup"><span data-stu-id="df336-229">Those can be viewed by swiping down from hello upper-left corner.</span></span>
   
       ![Testing on Android - notifications](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a><span data-ttu-id="df336-230">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df336-230">Next steps</span></span>
<span data-ttu-id="df336-231">Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers] oktatóanyagot hello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="df336-231">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step.</span></span> <span data-ttu-id="df336-232">Azt láthatja, hogyan toosend értesítést kapnak az ASP.NET háttérkiszolgáló használatával címkéket tootarget adott felhasználókra.</span><span class="sxs-lookup"><span data-stu-id="df336-232">It will show you how toosend notifications from an ASP.NET backend using tags tootarget specific users.</span></span>

<span data-ttu-id="df336-233">Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, tekintse meg a hello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="df336-233">If you want toosegment your users by interest groups, check out hello [Use Notification Hubs toosend breaking news] tutorial.</span></span>

<span data-ttu-id="df336-234">toolearn Notification Hubs további általános információt talál a [Notification Hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="df336-234">toolearn more general information about Notification Hubs, see our [Notification Hubs Guidance].</span></span>

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
