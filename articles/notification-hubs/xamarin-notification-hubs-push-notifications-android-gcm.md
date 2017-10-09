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
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a><span data-ttu-id="718ad-103">Ismerkedés a Notification Hubs Xamarin Android-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="718ad-103">Get started with Notification Hubs with Xamarin for Android</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="718ad-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="718ad-104">Overview</span></span>
<span data-ttu-id="718ad-105">Az oktatóanyag bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Xamarin.Android-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="718ad-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Xamarin.Android application.</span></span>
<span data-ttu-id="718ad-106">Létre fog hozni egy üres Xamarin.Android-alkalmazást, amely leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával.</span><span class="sxs-lookup"><span data-stu-id="718ad-106">You'll create a blank Xamarin.Android app that receives push notifications by using Google Cloud Messaging (GCM).</span></span> <span data-ttu-id="718ad-107">Amikor végzett, képes toouse lesz az értesítési központ toobroadcast leküldéses értesítések tooall hello eszközök az alkalmazást futtató.</span><span class="sxs-lookup"><span data-stu-id="718ad-107">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span> <span data-ttu-id="718ad-108">hello befejezett kód is elérhető hello [NotificationHubs-alkalmazásban] [ GitHub] minta.</span><span class="sxs-lookup"><span data-stu-id="718ad-108">hello finished code is available in hello [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="718ad-109">Ez az oktatóanyag bemutatja, hogyan hello egyszerű küldési forgatókönyvet a Notification Hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="718ad-109">This tutorial demonstrates hello simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="718ad-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="718ad-110">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="718ad-111">az oktatóanyag befejezése hello kódja a Githubon található [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span><span class="sxs-lookup"><span data-stu-id="718ad-111">hello completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="718ad-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="718ad-112">Prerequisites</span></span>
<span data-ttu-id="718ad-113">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="718ad-113">This tutorial requires hello following:</span></span>

* <span data-ttu-id="718ad-114">Windows rendszeren Visual Studio with Xamarin, vagy Mac OS X rendszeren Xamarin Studio. A teljes telepítési útmutatás itt található: [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).</span><span class="sxs-lookup"><span data-stu-id="718ad-114">Visual Studio with Xamarin on Windows or Xamarin Studio on Mac OS X. Complete installation instructions are on [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* <span data-ttu-id="718ad-115">Aktív Google-fiók</span><span class="sxs-lookup"><span data-stu-id="718ad-115">Active Google account</span></span>
* <span data-ttu-id="718ad-116">[Azure Messaging összetevő]</span><span class="sxs-lookup"><span data-stu-id="718ad-116">[Azure Messaging Component]</span></span>
* <span data-ttu-id="718ad-117">[Google Cloud Messaging Client összetevő]</span><span class="sxs-lookup"><span data-stu-id="718ad-117">[Google Cloud Messaging Client Component]</span></span>

<span data-ttu-id="718ad-118">Ennek az oktatóanyagnak az elvégzése előfeltétel minden további, Xamarin.Android-alkalmazásokkal kapcsolatos Notification Hubs-oktatóanyag elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="718ad-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin.Android apps.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="718ad-119">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="718ad-119">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="718ad-120">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="718ad-120">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="718ad-121">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="718ad-121">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).</span></span>
> 
> 

## <a name="enable-google-cloud-messaging"></a><span data-ttu-id="718ad-122">A Google Cloud Messaging engedélyezése</span><span class="sxs-lookup"><span data-stu-id="718ad-122">Enable Google Cloud Messaging</span></span>
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="718ad-123">Az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="718ad-123">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li><p><span data-ttu-id="718ad-124">Kattintson a hello <b>konfigurálása</b> hello felső lapra, adja meg a hello <b>API-kulcs</b> hello előző szakaszban beszerzett értékét, és kattintson a <b>mentése</b>.</span><span class="sxs-lookup"><span data-stu-id="718ad-124">Click hello <b>Configure</b> tab at hello top, enter hello <b>API Key</b> value you obtained in hello previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>
<span data-ttu-id="718ad-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span><span class="sxs-lookup"><span data-stu-id="718ad-125">&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)</span></span>

<span data-ttu-id="718ad-126">Az értesítési központ most konfigurált toowork GCM-mel, és az alkalmazás tooreceive értesítések és toosend leküldéses értesítések regisztrálása hello kapcsolati karakterláncok tooboth van.</span><span class="sxs-lookup"><span data-stu-id="718ad-126">Your notification hub is now configured toowork with GCM, and you have hello connection strings tooboth register your app tooreceive notifications and toosend push notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="718ad-127">Csatlakozás az alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="718ad-127">Connect your app toohello notification hub</span></span>
### <a name="create-a-new-project"></a><span data-ttu-id="718ad-128">Új projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="718ad-128">Create a new project</span></span>
1. <span data-ttu-id="718ad-129">A Xamarin Studióban kattintson a **New Solution** (Új megoldás), az **Android App** (Android-alkalmazás), majd a **Next** (Tovább) elemre.</span><span class="sxs-lookup"><span data-stu-id="718ad-129">In Xamarin Studio, click **New Solution**, click **Android App**, and click **Next**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. <span data-ttu-id="718ad-130">Adja meg az **App name** (Alkalmazás neve) és az **Identifier** (Azonosító) értékét.</span><span class="sxs-lookup"><span data-stu-id="718ad-130">Enter your **App Name** and **Identifier**.</span></span> <span data-ttu-id="718ad-131">Kattintson a hello **Target Plaforms** toosupport szeretné, majd kattintson **következő** és **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="718ad-131">Click hello **Target Plaforms** you want toosupport and then click **Next** and **Create**.</span></span>
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    <span data-ttu-id="718ad-132">Ezzel létrehoz egy új Android-projektet.</span><span class="sxs-lookup"><span data-stu-id="718ad-132">This creates a new Android project.</span></span>

1. <span data-ttu-id="718ad-133">Hello projekt tulajdonságainak megnyitásához kattintson a jobb gombbal az új projektre a hello megoldás nézet, és válasszon **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="718ad-133">Open hello project properties by right-clicking your new project in hello Solution view and choosing **Options**.</span></span> <span data-ttu-id="718ad-134">Jelölje be hello **Android-alkalmazás** hello elemére **Build** szakasz.</span><span class="sxs-lookup"><span data-stu-id="718ad-134">Select hello **Android Application** item in hello **Build** section.</span></span>
   
    <span data-ttu-id="718ad-135">Győződjön meg arról, hogy hello első betűjének a **csomagnév** értéke kisbetűvel kezdődik.</span><span class="sxs-lookup"><span data-stu-id="718ad-135">Ensure that hello first letter of your **Package name** is lowercase.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="718ad-136">hello hello csomagnév első betűjének kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="718ad-136">hello first letter of hello package name must be lowercase.</span></span> <span data-ttu-id="718ad-137">Különben az alkalmazásjegyzékkel kapcsolatos hibák lépnek fel a **BroadcastReceiver** és az **IntentFilter** leküldéses értesítésekre való alábbi regisztrálása során.</span><span class="sxs-lookup"><span data-stu-id="718ad-137">Otherwise, you will receive application manifest errors when you register your **BroadcastReceiver** and **IntentFilter** for push notifications below.</span></span>
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
2. <span data-ttu-id="718ad-138">Szükség esetén állítsa hello **az Android minimálisan** tooanother API-szintet.</span><span class="sxs-lookup"><span data-stu-id="718ad-138">Optionally, set hello **Minimum Android version** tooanother API Level.</span></span>
3. <span data-ttu-id="718ad-139">Szükség esetén állítsa hello **célja az Android** toohello, amelyet az tootarget (kell API-szintet 8-as vagy magasabb) egy másik API-verzió.</span><span class="sxs-lookup"><span data-stu-id="718ad-139">Optionally, set hello **Target Android version** toohello another API version that you want tootarget (must be API level 8 or higher).</span></span>

<span data-ttu-id="718ad-140">Kattintson a **OK** és Bezárás hello projekt beállításai párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="718ad-140">Click **OK** and close hello Project Options dialog.</span></span>

### <a name="add-hello-required-components-tooyour-project"></a><span data-ttu-id="718ad-141">Hello szükséges összetevők tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="718ad-141">Add hello required components tooyour project</span></span>
<span data-ttu-id="718ad-142">Google Cloud Messaging Client hello Xamarin Component Store áruházban elérhető hello egyszerűbben hello támogathatja a leküldéses értesítések támogatását a Xamarin.androidban.</span><span class="sxs-lookup"><span data-stu-id="718ad-142">hello Google Cloud Messaging Client available on hello Xamarin Component Store simplifies hello process of supporting push notifications in Xamarin.Android.</span></span>

1. <span data-ttu-id="718ad-143">Kattintson a jobb gombbal a hello összetevők mappára a Xamarin.Android-alkalmazásban, és válassza a **további összetevők beszerzése**.</span><span class="sxs-lookup"><span data-stu-id="718ad-143">Right-click hello Components folder in Xamarin.Android app and choose **Get More Components**.</span></span>
2. <span data-ttu-id="718ad-144">Keresse meg a hello **Azure Messaging** összetevő, és adja hozzá toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="718ad-144">Search for hello **Azure Messaging** component and add it toohello project.</span></span>
3. <span data-ttu-id="718ad-145">Keresse meg a hello **Google Cloud Messaging Client** összetevő, és adja hozzá toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="718ad-145">Search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span>

### <a name="set-up-notification-hubs-in-your-project"></a><span data-ttu-id="718ad-146">Értesítési központok beállítása a projektben</span><span class="sxs-lookup"><span data-stu-id="718ad-146">Set up notification hubs in your project</span></span>
1. <span data-ttu-id="718ad-147">Gyűjtse össze az Android alkalmazásra és az értesítési központ adatait a következő hello:</span><span class="sxs-lookup"><span data-stu-id="718ad-147">Gather hello following information for your Android app and notification hub:</span></span>
   
   * <span data-ttu-id="718ad-148">**GoogleProjectNumber**: a Projektszám értéke beszerezni az alkalmazást a Google fejlesztői portálján hello hello áttekintése.</span><span class="sxs-lookup"><span data-stu-id="718ad-148">**GoogleProjectNumber**:  Get this Project Number value from hello overview of your app on hello Google Developer Portal.</span></span> <span data-ttu-id="718ad-149">Feljegyezte korábbi ennek az értéknek hello portal hello alkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="718ad-149">You made a note of this value earlier when you created hello app on hello portal.</span></span>
   * <span data-ttu-id="718ad-150">**Figyelési kapcsolati karakterlánc**: hello irányítópulton a hello [klasszikus Azure portál], kattintson a **kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="718ad-150">**Listen connection string**: On hello dashboard in hello [Azure Classic Portal], click **View connection strings**.</span></span> <span data-ttu-id="718ad-151">Másolás hello *DefaultListenSharedAccessSignature* kapcsolati karakterláncot ezen értékhez.</span><span class="sxs-lookup"><span data-stu-id="718ad-151">Copy hello *DefaultListenSharedAccessSignature* connection string for this value.</span></span>
   * <span data-ttu-id="718ad-152">**Központnév**: hello a központ neve hello [klasszikus Azure portál].</span><span class="sxs-lookup"><span data-stu-id="718ad-152">**Hub name**: This is hello name of your hub from hello [Azure Classic Portal].</span></span> <span data-ttu-id="718ad-153">Például: *mynotificationhub2*.</span><span class="sxs-lookup"><span data-stu-id="718ad-153">For example, *mynotificationhub2*.</span></span>
     
     <span data-ttu-id="718ad-154">Hozzon létre egy **Constants.cs** osztályt a Xamarin-projekthez, és adja meg a következő állandó értékek hello osztályban hello.</span><span class="sxs-lookup"><span data-stu-id="718ad-154">Create a **Constants.cs** class for your Xamarin project and define hello following constant values in hello class.</span></span> <span data-ttu-id="718ad-155">Hello helyőrzőket cserélje le az értékeket.</span><span class="sxs-lookup"><span data-stu-id="718ad-155">Replace hello placeholders with your values.</span></span>
     
       <span data-ttu-id="718ad-156">public static class Constants   {</span><span class="sxs-lookup"><span data-stu-id="718ad-156">public static class Constants   {</span></span>
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
       <span data-ttu-id="718ad-157">}</span><span class="sxs-lookup"><span data-stu-id="718ad-157">}</span></span>
2. <span data-ttu-id="718ad-158">Adja hozzá hello következő using utasításokat túl**MainActivity.cs**:</span><span class="sxs-lookup"><span data-stu-id="718ad-158">Add hello following using statements too**MainActivity.cs**:</span></span>
   
        using Android.Util;
        using Gcm.Client;
3. <span data-ttu-id="718ad-159">Adja hozzá egy példány változó toohello `MainActivity` osztály, amely hello alkalmazás futtatásakor használt tooshow egy figyelmeztető párbeszédpanel lesz:</span><span class="sxs-lookup"><span data-stu-id="718ad-159">Add an instance variable toohello `MainActivity` class that will be used tooshow an alert dialog when hello app is running:</span></span>
   
        public static MainActivity instance;
4. <span data-ttu-id="718ad-160">Hozzon létre a következő metódus a hello hello **MainActivity** osztály:</span><span class="sxs-lookup"><span data-stu-id="718ad-160">Create hello following method in hello **MainActivity** class:</span></span>
   
        private void RegisterWithGCM()
        {
            // Check tooensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
5. <span data-ttu-id="718ad-161">A hello `OnCreate` metódusában **MainActivity.cs**, hello inicializálása `instance` változó, és adjon hozzá egy túl`RegisterWithGCM`:</span><span class="sxs-lookup"><span data-stu-id="718ad-161">In hello `OnCreate` method of **MainActivity.cs**, initialize hello `instance` variable and add a call too`RegisterWithGCM`:</span></span>
   
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
6. <span data-ttu-id="718ad-162">Hozzon létre az új **MyBroadcastReceiver** osztályt.</span><span class="sxs-lookup"><span data-stu-id="718ad-162">Create a new class, **MyBroadcastReceiver**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="718ad-163">Alább végigvezetjük a **BroadcastReceiver** osztály létrehozásának folyamatán az alapoktól kezdve.</span><span class="sxs-lookup"><span data-stu-id="718ad-163">We will walk through creating a **BroadcastReceiver** class from scratch below.</span></span> <span data-ttu-id="718ad-164">Azonban egy másik toomanually gyors létrehozása **MyBroadcastReceiver.cs** toorefer toohello van **GcmService.cs** fájl található a hello Xamarin.Android-projekt minta hello mellékelt[NotificationHubs-mintákban][GitHub].</span><span class="sxs-lookup"><span data-stu-id="718ad-164">However, a quick alternative toomanually creating **MyBroadcastReceiver.cs** is toorefer toohello **GcmService.cs** file found in hello sample Xamarin.Android project included with hello [NotificationHubs samples][GitHub].</span></span> <span data-ttu-id="718ad-165">Másolás **GcmService.cs** és osztálynevek módosítása egy remek toostart is lehet.</span><span class="sxs-lookup"><span data-stu-id="718ad-165">Duplicating **GcmService.cs** and changing class names can be a great place toostart as well.</span></span>
   > 
   > 
7. <span data-ttu-id="718ad-166">Adja hozzá hello következő using utasításokat túl**MyBroadcastReceiver.cs** (utaló toohello összetevőre és szerelvényre, korábban hozzáadott):</span><span class="sxs-lookup"><span data-stu-id="718ad-166">Add hello following using statements too**MyBroadcastReceiver.cs** (referring toohello component and assembly that you added earlier):</span></span>
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
8. <span data-ttu-id="718ad-167">A **MyBroadcastReceiver.cs**, adja hozzá a következő engedélyekre vonatkozó kérései között hello hello **használatával** utasítások és hello **névtér** deklarációjában:</span><span class="sxs-lookup"><span data-stu-id="718ad-167">In **MyBroadcastReceiver.cs**, add hello following permission requests between hello **using** statements and hello **namespace** declaration:</span></span>
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
9. <span data-ttu-id="718ad-168">A **MyBroadcastReceiver.cs**, módosítsa a hello **MyBroadcastReceiver** toomatch hello következő osztályban:</span><span class="sxs-lookup"><span data-stu-id="718ad-168">In **MyBroadcastReceiver.cs**, change hello **MyBroadcastReceiver** class toomatch hello following:</span></span>
   
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
10. <span data-ttu-id="718ad-169">A **MyBroadcastReceiver.cs** osztályban adjon hozzá egy másik, **PushHandlerService** nevű osztályt, amely a **GcmServiceBase** osztályból származik.</span><span class="sxs-lookup"><span data-stu-id="718ad-169">Add another class in **MyBroadcastReceiver.cs** named **PushHandlerService**, which derives from **GcmServiceBase**.</span></span> <span data-ttu-id="718ad-170">Győződjön meg arról, hogy tooapply hello **szolgáltatás** toohello attribútumosztály:</span><span class="sxs-lookup"><span data-stu-id="718ad-170">Make sure tooapply hello **Service** attribute toohello class:</span></span>
    
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
11. <span data-ttu-id="718ad-171">A **GcmServiceBase** az **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** és **OnError()** metódust valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="718ad-171">**GcmServiceBase** implements methods **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**, and **OnError()**.</span></span> <span data-ttu-id="718ad-172">A **PushHandlerService** megvalósítási osztálynak felül kell bírálnia ezeket a módszereket, és ezek a módszerek a válasz toointeracting hello értesítési központban fogja érvényesítést.</span><span class="sxs-lookup"><span data-stu-id="718ad-172">Our **PushHandlerService** implementation class must override these methods, and these methods will fire in response toointeracting with hello notification hub.</span></span>
12. <span data-ttu-id="718ad-173">Bírálja felül a hello **OnRegistered()** metódus a **PushHandlerService** hello kód a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="718ad-173">Override hello **OnRegistered()** method in **PushHandlerService** by using hello following code:</span></span>
    
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
    > <span data-ttu-id="718ad-174">A hello **OnRegistered()** fent kódját, vegye figyelembe a hello képességét toospecify címkék tooregister adott üzenetkezelési csatornák.</span><span class="sxs-lookup"><span data-stu-id="718ad-174">In hello **OnRegistered()** code above, you should note hello ability toospecify tags tooregister for specific messaging channels.</span></span>
    > 
    > 
13. <span data-ttu-id="718ad-175">Bírálja felül a hello **OnMessage** metódus a **PushHandlerService** hello kód a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="718ad-175">Override hello **OnMessage** method in **PushHandlerService** by using hello following code:</span></span>
    
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
14. <span data-ttu-id="718ad-176">Adja hozzá a következő hello **createNotification** és **dialogNotify** módszerek túl**PushHandlerService** a felhasználók értesítésére értesítés fogadásakor.</span><span class="sxs-lookup"><span data-stu-id="718ad-176">Add hello following **createNotification** and **dialogNotify** methods too**PushHandlerService** for notifying users when a notification is received.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="718ad-177">Az Android 5.0-s és újabb verzióiban az értesítések kialakítása jelentősen eltér a korábbi verzióktól.</span><span class="sxs-lookup"><span data-stu-id="718ad-177">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="718ad-178">Ha a tesztelést az Android 5.0-s vagy újabb, hello app tooreceive hello értesítésre futtató toobe kell.</span><span class="sxs-lookup"><span data-stu-id="718ad-178">If you test this on Android 5.0 or later, hello app will need toobe running tooreceive hello notification.</span></span> <span data-ttu-id="718ad-179">További információ: [Android-értesítések](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="718ad-179">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
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
15. <span data-ttu-id="718ad-180">Bírálja felül az **OnUnRegistered()**, **OnRecoverableError()** és **OnError()** absztrakt tagot a kód lefordításához:</span><span class="sxs-lookup"><span data-stu-id="718ad-180">Override abstract members **OnUnRegistered()**, **OnRecoverableError()**, and **OnError()** so that your code compiles:</span></span>
    
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

## <a name="run-your-app-in-hello-emulator"></a><span data-ttu-id="718ad-181">Az alkalmazás futtatása az emulátorban hello</span><span class="sxs-lookup"><span data-stu-id="718ad-181">Run your app in hello emulator</span></span>
<span data-ttu-id="718ad-182">Ha hello emulátorban futtatja az alkalmazást, győződjön meg arról, hogy egy Android virtuális eszközt (AVD), amely támogatja a Google API-kat használjon.</span><span class="sxs-lookup"><span data-stu-id="718ad-182">If you run this app in hello emulator, make sure that you use an Android Virtual Device (AVD) that supports Google APIs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="718ad-183">A sorrend tooreceive leküldéses értesítések be kell állítania egy Google-fiókba az Android virtuális eszközön.</span><span class="sxs-lookup"><span data-stu-id="718ad-183">In order tooreceive push notifications, you must set up a Google account on your Android Virtual Device.</span></span> <span data-ttu-id="718ad-184">(Hello emulátor, lépjen a túl**beállítások** kattintson **fiók hozzáadása**.) Győződjön meg arról, hogy hello emulátor csatlakoztatott toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="718ad-184">(In hello emulator, navigate too**Settings** and click **Add Account**.) Also, make sure that hello emulator is connected toohello Internet.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="718ad-185">Az Android 5.0-s és újabb verzióiban az értesítések kialakítása jelentősen eltér a korábbi verzióktól.</span><span class="sxs-lookup"><span data-stu-id="718ad-185">Notification design in Android version 5.0 and later represents a significant departure from that of previous versions.</span></span> <span data-ttu-id="718ad-186">További információ: [Android-értesítések](http://go.microsoft.com/fwlink/?LinkId=615880).</span><span class="sxs-lookup"><span data-stu-id="718ad-186">For more information, see [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880).</span></span>
> 
> 

1. <span data-ttu-id="718ad-187">A **Tools** (Eszközök) részen kattintson az **Open Android Emulator Manager** (Android-emulátorkezelő megnyitása) elemre, jelölje ki az eszközt, majd kattintson az **Edit** (Szerkesztés) gombra.</span><span class="sxs-lookup"><span data-stu-id="718ad-187">From **Tools**, click **Open Android Emulator Manager**, select your device, and then click **Edit**.</span></span>
   
      ![][18]
2. <span data-ttu-id="718ad-188">A **Target** (Cél) értékeként válassza a **Google APIs** (Google API-k) lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="718ad-188">Select **Google APIs** in **Target**, and then click **OK**.</span></span>
   
      ![][19]
3. <span data-ttu-id="718ad-189">A hello felső eszköztáron kattintson **futtatása**, majd válassza ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="718ad-189">On hello top toolbar, click **Run**, and then select your app.</span></span> <span data-ttu-id="718ad-190">Hello emulátor elindul, és a hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="718ad-190">This starts hello emulator and runs hello app.</span></span>
   
   <span data-ttu-id="718ad-191">hello alkalmazás lekéri hello *registrationId* a GCM és hello értesítési központ regisztrál.</span><span class="sxs-lookup"><span data-stu-id="718ad-191">hello app retrieves hello *registrationId* from GCM and registers with hello notification hub.</span></span>

## <a name="send-notifications-from-your-backend"></a><span data-ttu-id="718ad-192">Értesítések küldése a háttérrendszerből</span><span class="sxs-lookup"><span data-stu-id="718ad-192">Send notifications from your backend</span></span>
<span data-ttu-id="718ad-193">Értesítések fogadásának az alkalmazásban való értesítések a hello tesztelheti [klasszikus Azure portál] hello keresztül debug lapon hello értesítési központ, ahogy az alábbi üdvözlő képernyőt.</span><span class="sxs-lookup"><span data-stu-id="718ad-193">You can test receiving notifications in your app by sending notifications in hello [Azure Classic Portal] via hello debug tab on hello notification hub, as shown in hello screen below.</span></span>

![][30]

<span data-ttu-id="718ad-194">A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="718ad-194">Push notifications are normally sent in a backend service like Mobile Services or ASP.NET through a compatible library.</span></span> <span data-ttu-id="718ad-195">Hello REST API közvetlen toosend értesítési üzenetek, ha a szalagtár nem érhető el a kiszolgáló is használható.</span><span class="sxs-lookup"><span data-stu-id="718ad-195">You can also use hello REST API directly toosend notification messages if a library is not available for your backend.</span></span>

<span data-ttu-id="718ad-196">Egyéb oktatóprogramok találhatók, amelyeknél az értesítések küldésével felmerülhet tooreview listája itt található:</span><span class="sxs-lookup"><span data-stu-id="718ad-196">Here is a list of some other tutorials that you may want tooreview for sending notifications:</span></span>

* <span data-ttu-id="718ad-197">ASP.NET: Lásd: [Notification Hubs használata toopush értesítések toousers].</span><span class="sxs-lookup"><span data-stu-id="718ad-197">ASP.NET: See [Use Notification Hubs toopush notifications toousers].</span></span>
* <span data-ttu-id="718ad-198">Az Azure Notification Hubs Java SDK: lásd: [hogyan toouse Notification Hubs Java](notification-hubs-java-push-notification-tutorial.md) a küldhetők értesítések a Javával.</span><span class="sxs-lookup"><span data-stu-id="718ad-198">Azure Notification Hubs Java SDK: See [How toouse Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) for sending notifications from Java.</span></span> <span data-ttu-id="718ad-199">Ez az Eclipse-ben lett tesztelve Android-fejlesztéshez.</span><span class="sxs-lookup"><span data-stu-id="718ad-199">This has been tested in Eclipse for Android Development.</span></span>
* <span data-ttu-id="718ad-200">PHP: Lásd: [hogyan toouse php-ből a Notification Hubs](notification-hubs-php-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="718ad-200">PHP: See [How toouse Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

<span data-ttu-id="718ad-201">Hello hello oktatóanyag következő alszakaszaiban értesítéseket küld egy .NET-Konzolalkalmazás használatával, és egy mobilszolgáltatás használatával egy csomópontparancsfájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="718ad-201">In hello next subsections of hello tutorial, you send notifications by using a .NET console app, and by using a mobile service through a node script.</span></span>

#### <a name="optional-send-notifications-by-using-a-net-app"></a><span data-ttu-id="718ad-202">(Választható) Értesítések küldése .NET-alkalmazás használatával</span><span class="sxs-lookup"><span data-stu-id="718ad-202">(Optional) Send notifications by using a .NET app</span></span>
<span data-ttu-id="718ad-203">Ebben a szakaszban egy .NET-konzolalkalmazás használatával küldünk értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="718ad-203">In this section, we will send notifications by using a .NET console app</span></span>

1. <span data-ttu-id="718ad-204">Hozzon létre egy új Visual C#-konzolalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="718ad-204">Create a new Visual C# console application:</span></span>
   
      ![][20]
2. <span data-ttu-id="718ad-205">A Visual Studióban kattintson az **Eszközök**, a **NuGet Package Manager** (NuGet-csomagkezelő), majd a **Package Manager Console** (Csomagkezelő konzol) elemre.</span><span class="sxs-lookup"><span data-stu-id="718ad-205">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="718ad-206">Ez a Visual Studio hello Csomagkezelő konzol jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="718ad-206">This displays hello Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="718ad-207">Hello Package Manager Console ablakban, állítson be hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="718ad-207">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="718ad-208">Ezzel hozzáad egy hivatkozást toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.</span><span class="sxs-lookup"><span data-stu-id="718ad-208">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="718ad-209">Nyissa meg hello Program.cs fájlt, és adja hozzá a következő hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="718ad-209">Open hello Program.cs file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="718ad-210">Az a `Program` osztály, adja hozzá a következő metódus hello.</span><span class="sxs-lookup"><span data-stu-id="718ad-210">In your `Program` class, add hello following method.</span></span> <span data-ttu-id="718ad-211">Hello helyőrzők szövegét frissítése a *DefaultFullSharedAccessSignature* kapcsolati karakterláncot és a központ nevét a hello [klasszikus Azure portál].</span><span class="sxs-lookup"><span data-stu-id="718ad-211">Update hello placeholder text with your *DefaultFullSharedAccessSignature* connection string and hub name from hello [Azure Classic Portal].</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. <span data-ttu-id="718ad-212">Adja hozzá az alábbi hello a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="718ad-212">Add hello following lines in your **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="718ad-213">Nyomja le az hello F5 kulcs toorun hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="718ad-213">Press hello F5 key toorun hello app.</span></span> <span data-ttu-id="718ad-214">Hello alkalmazásban egy értesítést kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="718ad-214">You should receive a notification in hello app.</span></span>
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a><span data-ttu-id="718ad-215">(Választható) Értesítések küldése mobilszolgáltatás használatával</span><span class="sxs-lookup"><span data-stu-id="718ad-215">(Optional) Send notifications by using a mobile service</span></span>
1. <span data-ttu-id="718ad-216">Kövesse [A Mobile Services használatának első lépései] című témakör utasításait.</span><span class="sxs-lookup"><span data-stu-id="718ad-216">Follow [Get started with Mobile Services].</span></span>
2. <span data-ttu-id="718ad-217">Jelentkezzen be toohello [klasszikus Azure portál], és jelölje ki a mobilszolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="718ad-217">Sign in toohello [Azure Classic Portal], and select your mobile service.</span></span>
3. <span data-ttu-id="718ad-218">Jelölje be hello **Feladatütemező** hello legfelső lap.</span><span class="sxs-lookup"><span data-stu-id="718ad-218">Select hello **Scheduler** tab on hello top.</span></span>
   
      ![][22]
4. <span data-ttu-id="718ad-219">Hozzon létre egy új ütemezett feladatot, szúrjon be egy nevet, és válassza az **On demand** (Igény szerint) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="718ad-219">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
      ![][23]
5. <span data-ttu-id="718ad-220">Ha hello feladat jön létre, kattintson a hello feladat neve.</span><span class="sxs-lookup"><span data-stu-id="718ad-220">When hello job is created, click hello job name.</span></span> <span data-ttu-id="718ad-221">Kattintson a hello **parancsfájl** hello felső sávon fülre.</span><span class="sxs-lookup"><span data-stu-id="718ad-221">Then click hello **Script** tab on hello top bar.</span></span>
6. <span data-ttu-id="718ad-222">Helyezze be a következő parancsfájlt a scheduler függvényébe hello.</span><span class="sxs-lookup"><span data-stu-id="718ad-222">Insert hello following script inside your scheduler function.</span></span> <span data-ttu-id="718ad-223">Győződjön meg arról, hogy tooreplace hello helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultFullSharedAccessSignature* korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="718ad-223">Make sure tooreplace hello placeholders with your notification hub name and hello connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="718ad-224">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="718ad-224">Click **Save**.</span></span>
   
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
7. <span data-ttu-id="718ad-225">Kattintson a **egyszeri futtatás** hello alsó sáv.</span><span class="sxs-lookup"><span data-stu-id="718ad-225">Click **Run Once** on hello bottom bar.</span></span> <span data-ttu-id="718ad-226">Egy bejelentési értesítést kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="718ad-226">You should receive a toast notification.</span></span>

## <a name="next-steps"></a><span data-ttu-id="718ad-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="718ad-227">Next steps</span></span>
<span data-ttu-id="718ad-228">Ez az egyszerű példában küldött értesítések tooall az Android-eszközök.</span><span class="sxs-lookup"><span data-stu-id="718ad-228">In this simple example, you broadcasted notifications tooall your Android devices.</span></span> <span data-ttu-id="718ad-229">A rendezés tootarget adott felhasználók, tekintse meg a toohello oktatóanyag [Notification Hubs használata toopush értesítések toousers].</span><span class="sxs-lookup"><span data-stu-id="718ad-229">In order tootarget specific users, refer toohello tutorial [Use Notification Hubs toopush notifications toousers].</span></span> <span data-ttu-id="718ad-230">Ha azt szeretné, toosegment a felhasználókat érdeklődési körök, olvasható [legfrissebb hírek Notification Hubs használata toosend].</span><span class="sxs-lookup"><span data-stu-id="718ad-230">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="718ad-231">További tudnivalók toouse értesítési központok [Notification Hubs használatával] és hello [Notification Hubs útmutató-toofor Android].</span><span class="sxs-lookup"><span data-stu-id="718ad-231">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance] and in hello [Notification Hubs How-toofor Android].</span></span>

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
