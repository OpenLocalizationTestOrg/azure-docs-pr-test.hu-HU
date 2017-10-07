---
title: "aaaSending leküldéses értesítések tooAndroid az Azure Notification hubs használatával |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toopush értesítések tooAndroid eszközök."
services: notification-hubs
documentationcenter: android
keywords: "leküldéses értesítések,leküldéses értesítés,android leküldéses értesítés"
author: ysxu
manager: erikre
editor: 
ms.assetid: 8268c6ef-af63-433c-b14e-a20b04a0342a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/05/2016
ms.author: yuaxu
ms.openlocfilehash: 6b15a477d86cf1e6efffb21c5bcef0de7761af79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooandroid-with-azure-notification-hubs"></a><span data-ttu-id="30c8c-104">Az Azure Notification Hubs leküldéses értesítések tooAndroid küldése</span><span class="sxs-lookup"><span data-stu-id="30c8c-104">Sending push notifications tooAndroid with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="30c8c-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="30c8c-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="30c8c-106">Ez a témakör a leküldéses értesítések Google Cloud Messaging (GCM) használatával történő küldését mutatja be.</span><span class="sxs-lookup"><span data-stu-id="30c8c-106">This topic demonstrates push notifications with Google Cloud Messaging (GCM).</span></span> <span data-ttu-id="30c8c-107">Ha Google Firebase Cloud Messaging (FCM) használ, tekintse meg [küldő leküldéses értesítések tooAndroid az Azure Notification Hubs és FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="30c8c-107">If you are using Google's Firebase Cloud Messaging (FCM), see [Sending push notifications tooAndroid with Azure Notification Hubs and FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).</span></span>
> 
> 

<span data-ttu-id="30c8c-108">Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooan Android-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="30c8c-108">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooan Android application.</span></span>
<span data-ttu-id="30c8c-109">Létre fog hozni egy üres Android-alkalmazást, amely leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával.</span><span class="sxs-lookup"><span data-stu-id="30c8c-109">You'll create a blank Android app that receives push notifications by using Google Cloud Messaging (GCM).</span></span>

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="30c8c-110">az oktatóanyag kódjának befejeződött hello letölthető a GitHub [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="30c8c-110">hello completed code for this tutorial can be downloaded from GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30c8c-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="30c8c-111">Prerequisites</span></span>
> [!IMPORTANT]
> <span data-ttu-id="30c8c-112">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="30c8c-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="30c8c-113">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="30c8c-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="30c8c-114">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="30c8c-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).</span></span>
> 
> 

<span data-ttu-id="30c8c-115">Ezenkívül tooan aktív Azure-fiók említettük, ez az oktatóanyag csak hello legújabb verziója szükséges [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span><span class="sxs-lookup"><span data-stu-id="30c8c-115">In addition tooan active Azure account mentioned above, this tutorial only requires hello latest version of [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).</span></span>

<span data-ttu-id="30c8c-116">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Android-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="30c8c-116">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Android apps.</span></span>

## <a name="creating-a-project-that-supports-google-cloud-messaging"></a><span data-ttu-id="30c8c-117">Google Cloud Messaging szolgáltatást támogató projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="30c8c-117">Creating a project that supports Google Cloud Messaging</span></span>
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a><span data-ttu-id="30c8c-118">Új értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="30c8c-118">Configure a new notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="30c8c-119">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="30c8c-119">&emsp;&emsp;6.</span></span>   <span data-ttu-id="30c8c-120">A hello **beállítások** panelen válassza **értesítési szolgáltatások** , majd **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="30c8c-120">In hello **Settings** blade, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="30c8c-121">Adja meg hello API-kulcsot, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="30c8c-121">Enter hello API key and click **Save**.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

<span data-ttu-id="30c8c-123">Az értesítési központ már konfigurált toowork GCM-mel, és van hello kapcsolati karakterláncok tooboth regisztrálja az alkalmazást tooreceive, valamint leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="30c8c-123">Your notification hub is now configured toowork with GCM, and you have hello connection strings tooboth register your app tooreceive and send push notifications.</span></span>

## <span data-ttu-id="30c8c-124"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="30c8c-124"><a id="connecting-app"></a>Connect your app toohello notification hub</span></span>
### <a name="create-a-new-android-project"></a><span data-ttu-id="30c8c-125">Új Android-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="30c8c-125">Create a new Android project</span></span>
1. <span data-ttu-id="30c8c-126">Az Android Studióban indítson el egy új Android Studio-projektet.</span><span class="sxs-lookup"><span data-stu-id="30c8c-126">In Android Studio, start a new Android Studio project.</span></span>
   
   ![Android Studio – új projekt][13]
2. <span data-ttu-id="30c8c-128">Válassza ki a hello **telefon és táblagép** tényező és hello **minimális SDK** , amelyet az toosupport.</span><span class="sxs-lookup"><span data-stu-id="30c8c-128">Choose hello **Phone and Tablet** form factor and hello **Minimum SDK** that you want toosupport.</span></span> <span data-ttu-id="30c8c-129">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="30c8c-129">Then click **Next**.</span></span>
   
   ![Android Studio – projektlétrehozási munkafolyamat][14]
3. <span data-ttu-id="30c8c-131">Válasszon **üres tevékenység** hello fő tevékenység, kattintson a **következő**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="30c8c-131">Choose **Empty Activity** for hello main activity, click **Next**, and then click **Finish**.</span></span>

### <a name="add-google-play-services-toohello-project"></a><span data-ttu-id="30c8c-132">Google Play services toohello projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="30c8c-132">Add Google Play services toohello project</span></span>
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a><span data-ttu-id="30c8c-133">Azure Notification Hubs-kódtárak felvétele</span><span class="sxs-lookup"><span data-stu-id="30c8c-133">Adding Azure Notification Hubs libraries</span></span>
1. <span data-ttu-id="30c8c-134">A hello `Build.Gradle` hello fájlt **app**, adja hozzá a következő sorokat hello hello **függőségek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="30c8c-134">In hello `Build.Gradle` file for hello **app**, add hello following lines in hello **dependencies** section.</span></span>
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. <span data-ttu-id="30c8c-135">Adja hozzá a következő tárházat után hello hello **függőségek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="30c8c-135">Add hello following repository after hello **dependencies** section.</span></span>
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-hello-androidmanifestxml"></a><span data-ttu-id="30c8c-136">Hello AndroidManifest.xml frissítése.</span><span class="sxs-lookup"><span data-stu-id="30c8c-136">Updating hello AndroidManifest.xml.</span></span>
1. <span data-ttu-id="30c8c-137">toosupport GCM, létre kell hoznunk egy Példányazonosító-figyelő szolgáltatást a kódban, amely túl[regisztrációs jogkivonatok lekérésére](https://developers.google.com/cloud-messaging/android/client#sample-register) használatával [Google példányazonosító API](https://developers.google.com/instance-id/).</span><span class="sxs-lookup"><span data-stu-id="30c8c-137">toosupport GCM, we must implement a Instance ID listener service in our code which is used too[obtain registration tokens](https://developers.google.com/cloud-messaging/android/client#sample-register) using [Google's Instance ID API](https://developers.google.com/instance-id/).</span></span> <span data-ttu-id="30c8c-138">Az oktatóanyag azt hello osztály nevet `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="30c8c-138">In this tutorial we will name hello class `MyInstanceIDService`.</span></span> 
   
    <span data-ttu-id="30c8c-139">Adja hozzá a következő szolgáltatás definíciós toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="30c8c-139">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="30c8c-140">Cserélje le a hello `<your package>` helyőrzőt hello hello hello tetején látható tényleges csomagnévre `AndroidManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="30c8c-140">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>
2. <span data-ttu-id="30c8c-141">Miután GCM regisztrációs jogkivonat a hello példányazonosító API, ezzel túl[hello Azure Notification Hub regisztrálása](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="30c8c-141">Once we have received our GCM registration token from hello Instance ID API, we will use it too[register with hello Azure Notification Hub](notification-hubs-push-notification-registration-management.md).</span></span> <span data-ttu-id="30c8c-142">Ez a regisztráció támogatjuk hello háttér használatával egy `IntentService` nevű `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="30c8c-142">We will support this registration in hello background using an `IntentService` named `RegistrationIntentService`.</span></span> <span data-ttu-id="30c8c-143">Ez a szolgáltatás a [GCM regisztrációs jogkivonat frissítését](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) is el fogja végezni.</span><span class="sxs-lookup"><span data-stu-id="30c8c-143">This service will also be responsible for [refreshing our GCM registration token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).</span></span>
   
    <span data-ttu-id="30c8c-144">Adja hozzá a következő szolgáltatás definíciós toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="30c8c-144">Add hello following service definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="30c8c-145">Cserélje le a hello `<your package>` helyőrzőt hello hello hello tetején látható tényleges csomagnévre `AndroidManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="30c8c-145">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span> 
   
        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>
3. <span data-ttu-id="30c8c-146">A fogadó tooreceive értesítéseket is meghatározunk.</span><span class="sxs-lookup"><span data-stu-id="30c8c-146">We will also define a receiver tooreceive notifications.</span></span> <span data-ttu-id="30c8c-147">Adja hozzá a következő fogadó definition toohello AndroidManifest.xml fájl, belül hello hello `<application>` címke.</span><span class="sxs-lookup"><span data-stu-id="30c8c-147">Add hello following receiver definition toohello AndroidManifest.xml file, inside hello `<application>` tag.</span></span> <span data-ttu-id="30c8c-148">Cserélje le a hello `<your package>` helyőrzőt hello hello hello tetején látható tényleges csomagnévre `AndroidManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="30c8c-148">Replace hello `<your package>` placeholder with hello your actual package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="30c8c-149">Adja hozzá a következő szükséges GCM hello kapcsolatos alábbi hello engedélyek `</application>` címke.</span><span class="sxs-lookup"><span data-stu-id="30c8c-149">Add hello following necessary GCM related permissions below hello  `</application>` tag.</span></span> <span data-ttu-id="30c8c-150">Győződjön meg arról, hogy tooreplace `<your package>` hello hello tetején látható hello csomag nevű `AndroidManifest.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="30c8c-150">Make sure tooreplace `<your package>` with hello package name shown at hello top of hello `AndroidManifest.xml` file.</span></span>
   
    <span data-ttu-id="30c8c-151">További információk ezekről az engedélyekről: [GCM-ügyfélalkalmazás beállítása Androidhoz](https://developers.google.com/cloud-messaging/android/client#manifest).</span><span class="sxs-lookup"><span data-stu-id="30c8c-151">For more information on these permissions, see [Setup a GCM Client app for Android](https://developers.google.com/cloud-messaging/android/client#manifest).</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
   
        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>

### <a name="adding-code"></a><span data-ttu-id="30c8c-152">Kód felvétele</span><span class="sxs-lookup"><span data-stu-id="30c8c-152">Adding code</span></span>
1. <span data-ttu-id="30c8c-153">A projekt nézetben hello, bontsa ki a **app** > **src** > **fő** > **java**.</span><span class="sxs-lookup"><span data-stu-id="30c8c-153">In hello Project View, expand **app** > **src** > **main** > **java**.</span></span> <span data-ttu-id="30c8c-154">Kattintson a jobb gombbal a **java** területen látható csomagmappára, kattintson a **New** (Új), majd a **Java Class** (Java-osztály) elemre.</span><span class="sxs-lookup"><span data-stu-id="30c8c-154">Right-click your package folder under **java**, click **New**, and then click **Java Class**.</span></span> <span data-ttu-id="30c8c-155">Adjon hozzá egy új, `NotificationSettings` nevű osztályt.</span><span class="sxs-lookup"><span data-stu-id="30c8c-155">Add a new class named `NotificationSettings`.</span></span> 
   
    ![Android Studio – új Java-osztály][6]
   
    <span data-ttu-id="30c8c-157">Ellenőrizze, hogy tooupdate hello ezek hello hello kódját a következő három helyőrzőt `NotificationSettings` osztály:</span><span class="sxs-lookup"><span data-stu-id="30c8c-157">Make sure tooupdate hello these three placeholders in hello following code for hello `NotificationSettings` class:</span></span>
   
   * <span data-ttu-id="30c8c-158">**SenderId**: hello a korábban beszerzett Projektszám hello [Google Cloud Console](http://cloud.google.com/console).</span><span class="sxs-lookup"><span data-stu-id="30c8c-158">**SenderId**: hello project number you obtained earlier in hello [Google Cloud Console](http://cloud.google.com/console).</span></span>
   * <span data-ttu-id="30c8c-159">**HubListenConnectionString**: hello **DefaultListenAccessSignature** kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="30c8c-159">**HubListenConnectionString**: hello **DefaultListenAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="30c8c-160">Kapcsolati karakterlánc másolhatja kattintva **hozzáférési házirendek** a hello **beállítások** hello központ paneljén [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="30c8c-160">You can copy that connection string by clicking **Access Policies** on hello **Settings** blade of your hub on hello [Azure Portal].</span></span>
   * <span data-ttu-id="30c8c-161">**HubName**: hello név használata hello hello központ paneljén megjelenő értesítési központ [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="30c8c-161">**HubName**: Use hello name of your notification hub that appears in hello hub blade in hello [Azure Portal].</span></span>
     
     <span data-ttu-id="30c8c-162">`NotificationSettings` kód:</span><span class="sxs-lookup"><span data-stu-id="30c8c-162">`NotificationSettings` code:</span></span>
     
       <span data-ttu-id="30c8c-163">public class NotificationSettings {</span><span class="sxs-lookup"><span data-stu-id="30c8c-163">public class NotificationSettings {</span></span>
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Your default listen connection string>";
       <span data-ttu-id="30c8c-164">}</span><span class="sxs-lookup"><span data-stu-id="30c8c-164">}</span></span>
2. <span data-ttu-id="30c8c-165">Használatával a hello fent leírt lépésekkel, vegyen fel egy másik új osztályt `MyInstanceIDService`.</span><span class="sxs-lookup"><span data-stu-id="30c8c-165">Using hello steps above, add another new class named `MyInstanceIDService`.</span></span> <span data-ttu-id="30c8c-166">Ez lesz az általunk megvalósított Példányazonosító figyelőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="30c8c-166">This will be our Instance ID listener service implementation.</span></span>
   
    <span data-ttu-id="30c8c-167">Ez az osztály kódját hello szolgáltatás fel fogja hívni a `IntentService` túl[frissítési hello GCM-jogkivonat](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) hello háttérben.</span><span class="sxs-lookup"><span data-stu-id="30c8c-167">hello code for this class will call our `IntentService` too[refresh hello GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) in hello background.</span></span>
   
        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;

        public class MyInstanceIDService extends InstanceIDListenerService {

            private static final String TAG = "MyInstanceIDService";

            @Override
            public void onTokenRefresh() {

                Log.i(TAG, "Refreshing GCM Registration Token");

                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


1. <span data-ttu-id="30c8c-168">Adja hozzá egy másik új osztály tooyour nevű projekt, `RegistrationIntentService`.</span><span class="sxs-lookup"><span data-stu-id="30c8c-168">Add another new class tooyour project named, `RegistrationIntentService`.</span></span> <span data-ttu-id="30c8c-169">Ez lesz a hello megvalósítását a `IntentService` , amely elvégzi [frissítés hello GCM-jogkivonat](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) és [hello értesítési központ regisztrálása](notification-hubs-push-notification-registration-management.md).</span><span class="sxs-lookup"><span data-stu-id="30c8c-169">This will be hello implementation for our `IntentService` that will handle [refreshing hello GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) and [registering with hello notification hub](notification-hubs-push-notification-registration-management.md).</span></span>
   
    <span data-ttu-id="30c8c-170">Ez az osztály kódját a következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="30c8c-170">Use hello following code for this class.</span></span>
   
        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
   
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
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
   
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
   
                    // Storing hello registration id that indicates whether hello generated token has been
                    // sent tooyour server. If it is not stored, send hello token tooyour server,
                    // otherwise your server should have already received hello token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting tooregister with NH using token : " + token);
   
                        regID = hub.register(token).getRegistrationId();
   
                        // If you want toouse tags...
                        // Refer too: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();
   
                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed toocomplete token refresh", e);
                    // If an exception happens while fetching hello new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt hello update at a later time.
                }
   
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }
2. <span data-ttu-id="30c8c-171">Az a `MainActivity` osztály, adja hozzá a következő hello `import` fent hello utasítások osztály deklarációjában.</span><span class="sxs-lookup"><span data-stu-id="30c8c-171">In your `MainActivity` class, add hello following `import` statements above hello class declaration.</span></span>
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. <span data-ttu-id="30c8c-172">Adja hozzá az alábbi privát tagokat hello osztály hello tetején hello.</span><span class="sxs-lookup"><span data-stu-id="30c8c-172">Add hello following private members at hello top of hello class.</span></span> <span data-ttu-id="30c8c-173">Ezek használjuk [Google Play-szolgáltatások hello elérhetőségének ellenőrzése a Google által javasolt módon](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span><span class="sxs-lookup"><span data-stu-id="30c8c-173">We will use these [check hello availability of Google Play Services as recommended by Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).</span></span>
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. <span data-ttu-id="30c8c-174">Az a `MainActivity` osztály, adja hozzá a következő metódus toohello Google Play-szolgáltatások rendelkezésre állásának hello.</span><span class="sxs-lookup"><span data-stu-id="30c8c-174">In your `MainActivity` class, add hello following method toohello availability of Google Play Services.</span></span> 
   
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
5. <span data-ttu-id="30c8c-175">Az a `MainActivity` osztály, adja hozzá a következő kódot, amellyel a Google Play-szolgáltatások hívása előtt hello a `IntentService` tooget a GCM regisztrációs jogkivonat és regisztrálása az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="30c8c-175">In your `MainActivity` class, add hello following code that will check for Google Play Services before calling your `IntentService` tooget your GCM registration token and register with your notification hub.</span></span>
   
        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
   
            if (checkPlayServices()) {
                // Start IntentService tooregister this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. <span data-ttu-id="30c8c-176">A hello `OnCreate` hello metódusában `MainActivity` osztály, adja hozzá a hello tevékenység létrehozásakor a következő kód toostart hello regisztrációs folyamat során.</span><span class="sxs-lookup"><span data-stu-id="30c8c-176">In hello `OnCreate` method of hello `MainActivity` class, add hello following code toostart hello registration process when activity is created.</span></span>
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. <span data-ttu-id="30c8c-177">Adja hozzá a további módszereket toohello `MainActivity` tooverify alkalmazások állapotának és a jelentés az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="30c8c-177">Add these additional methods toohello `MainActivity` tooverify app state and report status in your app.</span></span>
   
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
8. <span data-ttu-id="30c8c-178">Hello `ToastNotify` metódusnak hello *"Hello World"* `TextView` tooreport állapot és az értesítések folyamatos jelentéséhez az hello app szabályozzák.</span><span class="sxs-lookup"><span data-stu-id="30c8c-178">hello `ToastNotify` method uses hello *"Hello World"* `TextView` control tooreport status and notifications persistently in hello app.</span></span> <span data-ttu-id="30c8c-179">Az activity_main.xml elrendezésben vegye fel az adott vezérlő azonosítója a következő hello.</span><span class="sxs-lookup"><span data-stu-id="30c8c-179">In your activity_main.xml layout, add hello following id for that control.</span></span>
   
       android:id="@+id/text_hello"
9. <span data-ttu-id="30c8c-180">A Tovább gombra a fogadó hello AndroidManifest.xml a meghatározott a adunk hozzá egy alosztályt.</span><span class="sxs-lookup"><span data-stu-id="30c8c-180">Next we will add a subclass for our receiver we defined in hello AndroidManifest.xml.</span></span> <span data-ttu-id="30c8c-181">Adja hozzá egy másik új osztály tooyour nevű projekt `MyHandler`.</span><span class="sxs-lookup"><span data-stu-id="30c8c-181">Add another new class tooyour project named `MyHandler`.</span></span>
10. <span data-ttu-id="30c8c-182">Adja hozzá a következő importálási utasításokat a felső hello hello `MyHandler.java`:</span><span class="sxs-lookup"><span data-stu-id="30c8c-182">Add hello following import statements at hello top of `MyHandler.java`:</span></span>
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. <span data-ttu-id="30c8c-183">Adja hozzá a következő kódot a hello hello `MyHandler` osztály, így a alosztálya `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span><span class="sxs-lookup"><span data-stu-id="30c8c-183">Add hello following code for hello `MyHandler` class making it a subclass of `com.microsoft.windowsazure.notifications.NotificationsHandler`.</span></span>
    
    <span data-ttu-id="30c8c-184">Ez a kód felülírja hello `OnReceive` metódust, így hello kezelő jelentést küld a kapott értesítésekről.</span><span class="sxs-lookup"><span data-stu-id="30c8c-184">This code overrides hello `OnReceive` method, so hello handler will report notifications that are received.</span></span> <span data-ttu-id="30c8c-185">hello értesítéskezelőnek is elküldi a hello leküldéses értesítési toohello Android értesítéskezelőjén hello segítségével `sendNotification()` metódust.</span><span class="sxs-lookup"><span data-stu-id="30c8c-185">hello handler also sends hello push notification toohello Android notification manager by using hello `sendNotification()` method.</span></span> <span data-ttu-id="30c8c-186">Hello `sendNotification()` metódust akkor kell végrehajtani, amikor hello alkalmazás nem fut, és értesítés érkezik.</span><span class="sxs-lookup"><span data-stu-id="30c8c-186">hello `sendNotification()` method should be executed when hello app is not running and a notification is received.</span></span>
    
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
12. <span data-ttu-id="30c8c-187">Az Android Studióban hello menüsávjában kattintson **Build** > **Rebuild Project** toomake meg arról, hogy a kód a jelen-e hibák.</span><span class="sxs-lookup"><span data-stu-id="30c8c-187">In Android Studio on hello menu bar, click **Build** > **Rebuild Project** toomake sure that no errors are present in your code.</span></span>

## <a name="sending-push-notifications"></a><span data-ttu-id="30c8c-188">Leküldéses értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="30c8c-188">Sending push notifications</span></span>
<span data-ttu-id="30c8c-189">Leküldéses értesítések fogadásának az alkalmazásban keresztül hello elküldésével tesztelheti [Azure Portal] -hello keressen **hibaelhárítás** hello központ paneljén szakasz alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="30c8c-189">You can test receiving push notifications in your app by sending them via hello [Azure Portal] - look for hello **Troubleshooting** Section in hello hub blade, as shown below.</span></span>

![Azure Notification Hubs – küldés tesztelése](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-hello-app"></a><span data-ttu-id="30c8c-191">(Választható) Leküldéses értesítések küldéséhez közvetlenül hello alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="30c8c-191">(Optional) Send push notifications directly from hello app</span></span>
<span data-ttu-id="30c8c-192">Az értesítések elküldése általában háttérkiszolgáló használatával történik.</span><span class="sxs-lookup"><span data-stu-id="30c8c-192">Normally, you would send notifications using a backend server.</span></span> <span data-ttu-id="30c8c-193">Bizonyos esetekben előfordulhat, hogy toobe képes toosend leküldéses értesítések közvetlenül ügyfélalkalmazástól hello keresi.</span><span class="sxs-lookup"><span data-stu-id="30c8c-193">For some cases, you might want toobe able toosend push notifications directly from hello client application.</span></span> <span data-ttu-id="30c8c-194">Ez a szakasz ismerteti, hogyan toosend értesítések hello ügyfélről hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="30c8c-194">This section explains how toosend notifications from hello client using hello [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).</span></span>

1. <span data-ttu-id="30c8c-195">Az Android Studio projektnézetében bontsa ki a következőt: **App** > **src** > **main** > **res** > **layout**.</span><span class="sxs-lookup"><span data-stu-id="30c8c-195">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **layout**.</span></span> <span data-ttu-id="30c8c-196">Nyissa meg hello `activity_main.xml` elrendezés fájlra, majd kattintson a hello **szöveg** tooupdate hello szöveg hello fájl tartalma fülre.</span><span class="sxs-lookup"><span data-stu-id="30c8c-196">Open hello `activity_main.xml` layout file and click hello **Text** tab tooupdate hello text contents of hello file.</span></span> <span data-ttu-id="30c8c-197">Frissítse hello kódot, amely hozzáadja az új `Button` és `EditText` küldési vezérlők leküldéses értesítési üzenetek toohello értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="30c8c-197">Update it with hello code below, which adds new `Button` and `EditText` controls for sending push notification messages toohello notification hub.</span></span> <span data-ttu-id="30c8c-198">Mielőtt csak hello lap alján, adja hozzá ezt a kódot `</RelativeLayout>`.</span><span class="sxs-lookup"><span data-stu-id="30c8c-198">Add this code at hello bottom, just before `</RelativeLayout>`.</span></span>
   
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
2. <span data-ttu-id="30c8c-199">Az Android Studio projektnézetében bontsa ki a következőt: **App** > **src** > **main** > **res** > **values**.</span><span class="sxs-lookup"><span data-stu-id="30c8c-199">In Android Studio Project View, expand **App** > **src** > **main** > **res** > **values**.</span></span> <span data-ttu-id="30c8c-200">Nyissa meg hello `strings.xml` fájlt, és új hello által hivatkozott hello karakterlánc-értékek `Button` és `EditText` szabályozza.</span><span class="sxs-lookup"><span data-stu-id="30c8c-200">Open hello `strings.xml` file and add hello string values that are referenced by hello new `Button` and `EditText` controls.</span></span> <span data-ttu-id="30c8c-201">Adja hozzá ezek hello fájl hello alján csak előtt `</resources>`.</span><span class="sxs-lookup"><span data-stu-id="30c8c-201">Add these at hello bottom of hello file, just before `</resources>`.</span></span>
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. <span data-ttu-id="30c8c-202">Az a `NotificationSetting.java` fájlt, adja hozzá a következő beállítás toohello hello `NotificationSettings` osztály.</span><span class="sxs-lookup"><span data-stu-id="30c8c-202">In your `NotificationSetting.java` file, add hello following setting toohello `NotificationSettings` class.</span></span>
   
    <span data-ttu-id="30c8c-203">Frissítés `HubFullAccess` a hello **DefaultFullSharedAccessSignature** kapcsolati karakterlánca.</span><span class="sxs-lookup"><span data-stu-id="30c8c-203">Update `HubFullAccess` with hello **DefaultFullSharedAccessSignature** connection string for your hub.</span></span> <span data-ttu-id="30c8c-204">Ez a kapcsolati karakterlánc átmásolhatók a hello [Azure Portal] kattintva **hozzáférési házirendek** a hello **beállítások** az értesítési központ paneljén.</span><span class="sxs-lookup"><span data-stu-id="30c8c-204">This connection string can be copied from hello [Azure Portal] by clicking **Access Policies** on hello **Settings** blade for your notification hub.</span></span>
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";
4. <span data-ttu-id="30c8c-205">Az a `MainActivity.java` fájlt, adja hozzá a következő hello `import` fent hello utasítások `MainActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="30c8c-205">In your `MainActivity.java` file, add hello following `import` statements above hello `MainActivity` class.</span></span>
   
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
5. <span data-ttu-id="30c8c-206">Az a `MainActivity.java` fájlt, adja hozzá a következő hello hello tetején a tagok hello `MainActivity` osztály.</span><span class="sxs-lookup"><span data-stu-id="30c8c-206">In your `MainActivity.java` file, add hello following members at hello top of hello `MainActivity` class.</span></span>    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. <span data-ttu-id="30c8c-207">Létre kell hoznia egy szoftverfrissítési hozzáférésű Jogosultságkód (SaS-) token tooauthenticate egy POST kérést toosend üzenetek tooyour értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="30c8c-207">You must create a Software Access Signature (SaS) token tooauthenticate a POST request toosend messages tooyour notification hub.</span></span> <span data-ttu-id="30c8c-208">Ehhez hello hello kapcsolati karakterlánc legfontosabb adatainak elemzése és SaS-jogkivonatot, majd hozza létre hello, ahogyan az hello [általánosan használt fogalmakat ismertető](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API-referenciában.</span><span class="sxs-lookup"><span data-stu-id="30c8c-208">This is done by parsing hello key data from hello connection string and then creating hello SaS token, as mentioned in hello [Common Concepts](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API reference.</span></span> <span data-ttu-id="30c8c-209">a következő kód hello egy megvalósítási példát szemléltet.</span><span class="sxs-lookup"><span data-stu-id="30c8c-209">hello following code is an example implementation.</span></span>
   
    <span data-ttu-id="30c8c-210">A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály tooparse a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="30c8c-210">In `MainActivity.java`, add hello following method toohello `MainActivity` class tooparse your connection string.</span></span>
   
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
7. <span data-ttu-id="30c8c-211">A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály toocreate egy SaS hitelesítési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="30c8c-211">In `MainActivity.java`, add hello following method toohello `MainActivity` class toocreate a SaS authentication token.</span></span>
   
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
8. <span data-ttu-id="30c8c-212">A `MainActivity.java`, adja hozzá a következő metódus toohello hello `MainActivity` osztály toohandle hello **értesítés küldése** gomb és üzenet toohello központ használatával hello beépített REST API hello leküldéses értesítést küldeni.</span><span class="sxs-lookup"><span data-stu-id="30c8c-212">In `MainActivity.java`, add hello following method toohello `MainActivity` class toohandle hello **Send Notification** button click and send hello push notification message toohello hub by using hello built-in REST API.</span></span>
   
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

## <a name="testing-your-app"></a><span data-ttu-id="30c8c-213">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="30c8c-213">Testing your app</span></span>
#### <a name="push-notifications-in-hello-emulator"></a><span data-ttu-id="30c8c-214">Leküldéses értesítések emulátorban hello</span><span class="sxs-lookup"><span data-stu-id="30c8c-214">Push notifications in hello emulator</span></span>
<span data-ttu-id="30c8c-215">Ha azt szeretné, hogy tootest leküldéses értesítések emulátorban, győződjön meg arról, hogy az emulátor rendszerképe támogatja-e a hello Google API-szintet az alkalmazás számára is választott.</span><span class="sxs-lookup"><span data-stu-id="30c8c-215">If you want tootest push notifications inside an emulator, make sure that your emulator image supports hello Google API level that you chose for your app.</span></span> <span data-ttu-id="30c8c-216">Ha a lemezkép nem támogatja a natív Google API-k, akkor rendszer végül hello **szolgáltatás\_nem\_elérhető** kivétel.</span><span class="sxs-lookup"><span data-stu-id="30c8c-216">If your image doesn't support native Google APIs, you will end up with hello **SERVICE\_NOT\_AVAILABLE** exception.</span></span>

<span data-ttu-id="30c8c-217">Ezenkívül toohello fenti, győződjön meg arról, hogy hozzáadta a Google-fiók az emulátor futtató tooyour **beállítások** > **fiókok**.</span><span class="sxs-lookup"><span data-stu-id="30c8c-217">In addition toohello above, ensure that you have added your Google account tooyour running emulator under **Settings** > **Accounts**.</span></span> <span data-ttu-id="30c8c-218">Ellenkező esetben a GCM-mel kísérletek tooregister hello eredményezhet **hitelesítési\_sikertelen** kivétel.</span><span class="sxs-lookup"><span data-stu-id="30c8c-218">Otherwise, your attempts tooregister with GCM may result in hello **AUTHENTICATION\_FAILED** exception.</span></span>

#### <a name="running-hello-application"></a><span data-ttu-id="30c8c-219">Hello alkalmazást futtat</span><span class="sxs-lookup"><span data-stu-id="30c8c-219">Running hello application</span></span>
1. <span data-ttu-id="30c8c-220">Hello alkalmazás futtatását, és figyelje meg, hogy sikeres regisztráció jelentett-e hello regisztrációs Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="30c8c-220">Run hello app and notice that hello registration ID is reported for a successful registration.</span></span>
   
      ![Tesztelés Android rendszeren – Csatornaregisztráció][18]
2. <span data-ttu-id="30c8c-222">Adjon meg egy értesítési üzenetet toobe tooall Android-eszközök hello központon regisztrált küldött.</span><span class="sxs-lookup"><span data-stu-id="30c8c-222">Enter a notification message toobe sent tooall Android devices that have registered with hello hub.</span></span>
   
      ![Tesztelés Android rendszeren – Üzenetküldés][19]

3. <span data-ttu-id="30c8c-224">Nyomja le a **Send Notification** (Értesítés küldése) gombot.</span><span class="sxs-lookup"><span data-stu-id="30c8c-224">Press **Send Notification**.</span></span> <span data-ttu-id="30c8c-225">Hello app futó összes eszközön megjelenik egy `AlertDialog` példány hello leküldéses értesítési üzenettel.</span><span class="sxs-lookup"><span data-stu-id="30c8c-225">Any devices that have hello app running will show an `AlertDialog` instance with hello push notification message.</span></span> <span data-ttu-id="30c8c-226">A leküldéses értesítések, amelyek nem rendelkeznek hello alkalmazás fut, de korábban már regisztrált eszközök az Android Értesítéskezelőjén hello egy értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="30c8c-226">Devices that don't have hello app running but were previously registered for push notifications will receive a notification in hello Android Notification Manager.</span></span> <span data-ttu-id="30c8c-227">Azok az hello bal felső sarokból lefelé pöccintve tekinthetők meg.</span><span class="sxs-lookup"><span data-stu-id="30c8c-227">Those can be viewed by swiping down from hello upper-left corner.</span></span>
   
      ![Tesztelés Android rendszeren – Értesítések][21]

## <a name="next-steps"></a><span data-ttu-id="30c8c-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="30c8c-229">Next steps</span></span>
<span data-ttu-id="30c8c-230">Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers] oktatóanyagot hello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="30c8c-230">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step.</span></span> <span data-ttu-id="30c8c-231">Azt láthatja, hogyan toosend értesítést kapnak az ASP.NET háttérkiszolgáló használatával címkéket tootarget adott felhasználókra.</span><span class="sxs-lookup"><span data-stu-id="30c8c-231">It will show you how toosend notifications from an ASP.NET backend using tags tootarget specific users.</span></span>

<span data-ttu-id="30c8c-232">Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, tekintse meg a hello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="30c8c-232">If you want toosegment your users by interest groups, check out hello [Use Notification Hubs toosend breaking news] tutorial.</span></span>

<span data-ttu-id="30c8c-233">toolearn Notification Hubs további általános információt talál a [Notification Hubs használatával].</span><span class="sxs-lookup"><span data-stu-id="30c8c-233">toolearn more general information about Notification Hubs, see our [Notification Hubs Guidance].</span></span>

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs használata toopush értesítések toousers]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[legfrissebb hírek Notification Hubs használata toosend]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
