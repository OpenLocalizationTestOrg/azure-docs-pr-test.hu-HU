---
title: "aaaAdd leküldéses értesítések tooApache Cordova-alkalmazás az Azure Mobile Apps |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Mobile Apps toosend leküldéses értesítések tooyour Apache Cordova-alkalmazáshoz."
services: app-service\mobile
documentationcenter: javascript
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 8e1b23d6145b446b6f01599337b677e2f2b31d7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-apache-cordova-app"></a><span data-ttu-id="78623-103">Leküldéses értesítések tooyour Apache Cordova-alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="78623-103">Add push notifications tooyour Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="78623-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="78623-104">Overview</span></span>
<span data-ttu-id="78623-105">Ebben az oktatóanyagban hozzáadja leküldéses értesítések toohello [Apache Cordova gyors üzembe helyezési] projekt, hogy egy leküldéses értesítést küld toohello eszköz minden alkalommal, amikor egy olyan rekordot csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="78623-105">In this tutorial, you add push notifications toohello [Apache Cordova quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="78623-106">Ha nem használ hello letöltése – első lépések, leküldéses értesítési kiterjesztési csomagot kell hello.</span><span class="sxs-lookup"><span data-stu-id="78623-106">If you do not use hello downloaded quick start server project, you need hello push notification extension package.</span></span> <span data-ttu-id="78623-107">További információkért lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a][1].</span><span class="sxs-lookup"><span data-stu-id="78623-107">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps][1].</span></span>

## <span data-ttu-id="78623-108"><a name="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="78623-108"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="78623-109">Ez az oktatóanyag hozzá van rendelve egy, a Visual Studio 2015-öt hello Google Android Emulator, Android-eszközön, a Windows-eszköz és az iOS-eszközök futó létrehozott Apache Cordova-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="78623-109">This tutorial covers an Apache Cordova application developed with Visual Studio 2015 that runs on hello Google Android Emulator, an Android device, a Windows device, and an iOS device.</span></span>

<span data-ttu-id="78623-110">toocomplete ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="78623-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="78623-111">Számítógép, amelyen fut [Visual Studio Community 2015] [ 2] vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="78623-111">A PC with [Visual Studio Community 2015][2] or later versions.</span></span>
* <span data-ttu-id="78623-112">[Visual Studio Tools for Apache Cordova][4].</span><span class="sxs-lookup"><span data-stu-id="78623-112">[Visual Studio Tools for Apache Cordova][4].</span></span>
* <span data-ttu-id="78623-113">Egy [aktív Azure-fiók][3].</span><span class="sxs-lookup"><span data-stu-id="78623-113">An [active Azure account][3].</span></span>
* <span data-ttu-id="78623-114">A befejezett [Apache Cordova gyors üzembe helyezési] [ 5] projekt.</span><span class="sxs-lookup"><span data-stu-id="78623-114">A completed [Apache Cordova quick start][5] project.</span></span>
* <span data-ttu-id="78623-115">(Android) A [Google-fiók] [ 6] egy hitelesített e-mail címmel.</span><span class="sxs-lookup"><span data-stu-id="78623-115">(Android) A [Google account][6] with a verified email address.</span></span>
* <span data-ttu-id="78623-116">(csak iOS) Egy [Apple Fejlesztőprogrambeli tagság] [ 7] és egy iOS-eszközön (iOS-szimulátor nem támogatja a leküldéses).</span><span class="sxs-lookup"><span data-stu-id="78623-116">(iOS) An [Apple Developer Program membership][7] and an iOS device (iOS Simulator does not support push).</span></span>
* <span data-ttu-id="78623-117">(Windows) A [Windows áruház fejlesztői fiókjába] [ 8] és a Windows 10-eszközre.</span><span class="sxs-lookup"><span data-stu-id="78623-117">(Windows) A [Windows Store Developer Account][8] and a Windows 10 device.</span></span>

## <span data-ttu-id="78623-118"><a name="configure-hub"></a>Egy értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78623-118"><a name="configure-hub"></a>Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

<span data-ttu-id="78623-119">[Ebben a szakaszban lépéseket bemutató videó megtekintése][9]</span><span class="sxs-lookup"><span data-stu-id="78623-119">[Watch a video showing steps in this section][9]</span></span>

## <a name="update-hello-server-project"></a><span data-ttu-id="78623-120">Hello server projekt frissítése</span><span class="sxs-lookup"><span data-stu-id="78623-120">Update hello server project</span></span>
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <span data-ttu-id="78623-121"><a name="add-push-to-app"></a>Módosítsa a Cordova-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="78623-121"><a name="add-push-to-app"></a>Modify your Cordova app</span></span>
<span data-ttu-id="78623-122">Győződjön meg a Apache Cordova-alkalmazás projekt telepítése hello Cordova leküldéses beépülő modul és a platform-specifikus leküldéses szolgáltatások által készen toohandle leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="78623-122">Ensure your Apache Cordova app project is ready toohandle push notifications by installing hello Cordova push plugin plus any platform-specific push services.</span></span>

#### <a name="update-hello-cordova-version-in-your-project"></a><span data-ttu-id="78623-123">A projekt hello Cordova-verzió frissítése.</span><span class="sxs-lookup"><span data-stu-id="78623-123">Update hello Cordova version in your project.</span></span>
<span data-ttu-id="78623-124">A projekt Apache Cordova v6.1.1-nél korábbi verzióját használja, ha hello ügyfél projekt frissítése.</span><span class="sxs-lookup"><span data-stu-id="78623-124">If your project uses a version of Apache Cordova earlier than v6.1.1, update hello client project.</span></span> <span data-ttu-id="78623-125">a projekt hello tooupdate:</span><span class="sxs-lookup"><span data-stu-id="78623-125">tooupdate hello project:</span></span>

* <span data-ttu-id="78623-126">Kattintson a jobb gombbal `config.xml` tooopen hello configuration designer.</span><span class="sxs-lookup"><span data-stu-id="78623-126">Right-click `config.xml` tooopen hello configuration designer.</span></span>
* <span data-ttu-id="78623-127">Válassza ki a hello platformok fülre.</span><span class="sxs-lookup"><span data-stu-id="78623-127">Select hello Platforms tab.</span></span>
* <span data-ttu-id="78623-128">Adja meg 6.1.1 hello **Cordova CLI** szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="78623-128">Choose 6.1.1 in hello **Cordova CLI** text box.</span></span>
* <span data-ttu-id="78623-129">Válasszon **Build**, majd **megoldás fordítása** tooupdate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="78623-129">Choose **Build**, then **Build Solution** tooupdate hello project.</span></span>

#### <a name="install-hello-push-plugin"></a><span data-ttu-id="78623-130">Hello leküldéses beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="78623-130">Install hello push plugin</span></span>
<span data-ttu-id="78623-131">Apache Cordova-alkalmazás natív módon nem kezeli a eszköz vagy a hálózatkezelő képességeit.</span><span class="sxs-lookup"><span data-stu-id="78623-131">Apache Cordova applications do not natively handle device or network capabilities.</span></span>  <span data-ttu-id="78623-132">Ezek a képességek által biztosított beépülő modulok, amelyek közzé vagy [npm] [ 10] vagy a Githubon.</span><span class="sxs-lookup"><span data-stu-id="78623-132">These capabilities are provided by plugins that are published either on [npm][10] or on GitHub.</span></span>  <span data-ttu-id="78623-133">Hello `phonegap-plugin-push` beépülő modul használt toohandle hálózati leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="78623-133">hello `phonegap-plugin-push` plugin is used toohandle network push notifications.</span></span>

<span data-ttu-id="78623-134">Hello leküldéses beépülő modul az alábbi módszerek egyikével telepítheti:</span><span class="sxs-lookup"><span data-stu-id="78623-134">You can install hello push plugin in one of these ways:</span></span>

<span data-ttu-id="78623-135">**Hello-parancssorból:**</span><span class="sxs-lookup"><span data-stu-id="78623-135">**From hello command-prompt:**</span></span>

<span data-ttu-id="78623-136">A következő parancs hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="78623-136">Execute hello following command:</span></span>

    cordova plugin add phonegap-plugin-push

<span data-ttu-id="78623-137">**A Visual studióban:**</span><span class="sxs-lookup"><span data-stu-id="78623-137">**From within Visual Studio:**</span></span>

1. <span data-ttu-id="78623-138">A Solution Explorerben nyissa meg a hello `config.xml` fájl kattintson **beépülő modulok** > **egyéni**, jelölje be **Git** telepítési forrásként, majd adja meg `https://github.com/phonegap/phonegap-plugin-push`hello forrásaként.</span><span class="sxs-lookup"><span data-stu-id="78623-138">In Solution Explorer, open hello `config.xml` file click **Plugins** > **Custom**, select **Git** as the  installation source, then enter `https://github.com/phonegap/phonegap-plugin-push` as hello source.</span></span>

   ![][img1]

2. <span data-ttu-id="78623-139">Kattintson a hello nyíl következő toohello telepítési forrás.</span><span class="sxs-lookup"><span data-stu-id="78623-139">Click hello arrow next toohello installation source.</span></span>
3. <span data-ttu-id="78623-140">A **SENDER_ID**, ha már rendelkezik egy numerikus Projektazonosítónak hello Google Developer Console projekt, hozzáadhatja azt itt.</span><span class="sxs-lookup"><span data-stu-id="78623-140">In **SENDER_ID**, if you already have a numeric project ID for hello Google Developer Console project, you can add it here.</span></span> <span data-ttu-id="78623-141">Ellenkező esetben adja meg a helyőrző érték, például 777777.</span><span class="sxs-lookup"><span data-stu-id="78623-141">Otherwise, enter a placeholder value, like 777777.</span></span>  <span data-ttu-id="78623-142">Android céloz meg, ha ezt az értéket config.xml később frissítheti.</span><span class="sxs-lookup"><span data-stu-id="78623-142">If you are targeting Android, you can update this value in config.xml later.</span></span>
4. <span data-ttu-id="78623-143">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="78623-143">Click **Add**.</span></span>

<span data-ttu-id="78623-144">hello leküldéses beépülő modul telepítve van.</span><span class="sxs-lookup"><span data-stu-id="78623-144">hello push plugin is now installed.</span></span>

#### <a name="install-hello-device-plugin"></a><span data-ttu-id="78623-145">Hello eszköz beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="78623-145">Install hello device plugin</span></span>
<span data-ttu-id="78623-146">Hajtsa végre az azonos hello tooinstall hello leküldéses beépülő modul használt művelet.</span><span class="sxs-lookup"><span data-stu-id="78623-146">Follow hello same procedure you used tooinstall hello push plugin.</span></span>  <span data-ttu-id="78623-147">Hello eszköz beépülő modul hozzáadása hello Core beépülő modulok listában (kattintson **beépülő modulok** > **Core** toofind azt).</span><span class="sxs-lookup"><span data-stu-id="78623-147">Add hello Device plugin from hello Core plugins list (click **Plugins** > **Core** toofind it).</span></span> <span data-ttu-id="78623-148">A beépülő modul tooobtain hello platform név van szüksége.</span><span class="sxs-lookup"><span data-stu-id="78623-148">You need this plugin tooobtain hello platform name.</span></span>

#### <a name="register-your-device-on-application-start-up"></a><span data-ttu-id="78623-149">Regisztrálja az eszközt, az alkalmazás indítási</span><span class="sxs-lookup"><span data-stu-id="78623-149">Register your device on application start-up</span></span>
<span data-ttu-id="78623-150">Kezdetben magában foglalja az egyes minimális kód Android rendszerhez.</span><span class="sxs-lookup"><span data-stu-id="78623-150">Initially, we include some minimal code for Android.</span></span> <span data-ttu-id="78623-151">Hello app toorun iOS-vagy Windows 10 később módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="78623-151">Later, modify hello app toorun on iOS or Windows 10.</span></span>

1. <span data-ttu-id="78623-152">Adjon hozzá egy túl**registerForPushNotifications** hello bejelentkezési folyamat vagy hello hello alján hello visszahívás során **onDeviceReady** módszert:</span><span class="sxs-lookup"><span data-stu-id="78623-152">Add a call too**registerForPushNotifications** during hello callback for hello login process, or at hello bottom of  hello **onDeviceReady** method:</span></span>

        // Login toohello service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh hello todoItems
                refreshDisplay();

                // Wire up hello UI Event Handler for hello Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added tooregister for push notifications.
                registerForPushNotifications();

            }, handleError);

    <span data-ttu-id="78623-153">Ez a példa bemutatja hívása **registerForPushNotifications** sikeres hitelesítést követően.</span><span class="sxs-lookup"><span data-stu-id="78623-153">This example shows calling **registerForPushNotifications** after authentication succeeds.</span></span>  <span data-ttu-id="78623-154">Hívása `registerForPushNotifications()` gyakran szükség.</span><span class="sxs-lookup"><span data-stu-id="78623-154">You can call `registerForPushNotifications()` as often as is required.</span></span>

2. <span data-ttu-id="78623-155">Adja hozzá az új hello **registerForPushNotifications** módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="78623-155">Add hello new **registerForPushNotifications** method as follows:</span></span>

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle hello registration event.
        pushRegistration.on('registration', function (data) {
          // Get hello native platform of hello device.
          var platform = device.platform;
          // Get hello handle returned during registration.
          var handle = data.registrationId;
          // Set hello device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }
3. <span data-ttu-id="78623-156">(Android) Cserélje le a hello megelőző kódot, `Your_Project_ID` hello numerikus project azonosítója a az alkalmazás a [Google Developer Console][18].</span><span class="sxs-lookup"><span data-stu-id="78623-156">(Android) In hello preceding code, replace `Your_Project_ID` with hello numeric project ID for your app from the  [Google Developer Console][18].</span></span>

## <a name="optional-configure-and-run-hello-app-on-android"></a><span data-ttu-id="78623-157">(Választható) Konfigurálása és az Android hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="78623-157">(Optional) Configure and run hello app on Android</span></span>
<span data-ttu-id="78623-158">Ez a szakasz tooenable leküldéses értesítések Android befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="78623-158">Complete this section tooenable push notifications for Android.</span></span>

#### <span data-ttu-id="78623-159"><a name="enable-gcm"></a>Engedélyezze a Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="78623-159"><a name="enable-gcm"></a>Enable Firebase Cloud Messaging</span></span>
<span data-ttu-id="78623-160">Mivel kezdetben vannak célzó hello Google Android platform, engedélyeznie kell a Firebase Cloud Messaging.</span><span class="sxs-lookup"><span data-stu-id="78623-160">Since we are targeting hello Google Android platform initially, you must enable Firebase Cloud Messaging.</span></span>

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <span data-ttu-id="78623-161"><a name="configure-backend"></a>Hello mobilalkalmazás háttér toosend leküldéses kérelmek FCM történő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78623-161"><a name="configure-backend"></a>Configure hello Mobile App backend toosend push requests using FCM</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a><span data-ttu-id="78623-162">Konfigurálja a Cordova-alkalmazás Android rendszerhez</span><span class="sxs-lookup"><span data-stu-id="78623-162">Configure your Cordova app for Android</span></span>
<span data-ttu-id="78623-163">A Cordova-alkalmazáshoz, nyissa meg a config.xml, és cserélje le `Your_Project_ID` hello numerikus project azonosítója az alkalmazás a hello [Google Developer Console][18].</span><span class="sxs-lookup"><span data-stu-id="78623-163">In your Cordova app, open config.xml and replace `Your_Project_ID` with hello numeric project ID for your app from hello [Google Developer Console][18].</span></span>

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

<span data-ttu-id="78623-164">Nyissa meg a index.js és hello kód toouse frissítése a numerikus projekt ID azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="78623-164">Open index.js and update hello code toouse your numeric project ID.</span></span>

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <span data-ttu-id="78623-165"><a name="configure-device"></a>Android-eszköz az USB-hibakeresés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78623-165"><a name="configure-device"></a>Configure your Android device for USB debugging</span></span>
<span data-ttu-id="78623-166">Az alkalmazás tooyour Android-eszköz telepítése előtt kell tooenable USB-hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="78623-166">Before you can deploy your application tooyour Android Device, you need tooenable USB Debugging.</span></span>  <span data-ttu-id="78623-167">Androidos telefonja hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="78623-167">Perform the following steps on your Android phone:</span></span>

1. <span data-ttu-id="78623-168">Nyissa meg túl**beállítások** > **névjegye**, koppintson a hello **buildszáma** mindaddig, amíg a fejlesztői mód engedélyezve van (körülbelül hét alkalommal).</span><span class="sxs-lookup"><span data-stu-id="78623-168">Go too**Settings** > **About phone**, then tap hello **Build number** until developer mode is enabled  (about seven times).</span></span>
2. <span data-ttu-id="78623-169">Vissza a **beállítások** > **fejlesztői beállítások** engedélyezése **USB-hibakeresés**, majd csatlakoztassa az Android-telefonon tooyour fejlesztési számítógép USB-kábellel.</span><span class="sxs-lookup"><span data-stu-id="78623-169">Back in **Settings** > **Developer Options** enable **USB debugging**, then connect your Android phone  tooyour development PC with a USB Cable.</span></span>

<span data-ttu-id="78623-170">Tesztelt Ez egy Android 6.0 (Marshmallow) rendszert futtató Google Nexus 5 X-eszköz használatával.</span><span class="sxs-lookup"><span data-stu-id="78623-170">We tested this using a Google Nexus 5X device running Android 6.0 (Marshmallow).</span></span>  <span data-ttu-id="78623-171">Hello technikák azonban által közösen bármelyik modern Android verzióját.</span><span class="sxs-lookup"><span data-stu-id="78623-171">However, hello techniques are common across any modern Android release.</span></span>

#### <a name="install-google-play-services"></a><span data-ttu-id="78623-172">Google Play-szolgáltatások telepítése</span><span class="sxs-lookup"><span data-stu-id="78623-172">Install Google Play Services</span></span>
<span data-ttu-id="78623-173">hello leküldéses beépülő modul az Android Google Play-szolgáltatások a leküldéses értesítések támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="78623-173">hello push plugin relies on Android Google Play Services for push notifications.</span></span>

1. <span data-ttu-id="78623-174">A Visual Studióban kattintson **eszközök** > **Android** > **Android SDK Manager**, bontsa ki a hello **kiegészítő funkciók** mappa és ellenőrzés hello mezőben toomake a következő SDK-k hello mindegyikének telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="78623-174">In Visual Studio, click **Tools** > **Android** > **Android SDK Manager**, expand hello **Extras** folder and  check hello box toomake sure that each of hello following SDKs is installed.</span></span>

   * <span data-ttu-id="78623-175">Android 2.3-as vagy újabb</span><span class="sxs-lookup"><span data-stu-id="78623-175">Android 2.3 or higher</span></span>
   * <span data-ttu-id="78623-176">Google-tárház változat 27 vagy nagyobb</span><span class="sxs-lookup"><span data-stu-id="78623-176">Google Repository revision 27 or higher</span></span>
   * <span data-ttu-id="78623-177">Google Play-szolgáltatásokra 9.0.2 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="78623-177">Google Play Services 9.0.2 or higher</span></span>

2. <span data-ttu-id="78623-178">Kattintson a **csomagok telepítése** és hello telepítési toocomplete várja.</span><span class="sxs-lookup"><span data-stu-id="78623-178">Click **Install Packages** and wait for hello installation toocomplete.</span></span>

<span data-ttu-id="78623-179">hello aktuális szükséges kódtárak hello szereplő [phonegap-beépülőmodul-leküldéses telepítési dokumentáció][19].</span><span class="sxs-lookup"><span data-stu-id="78623-179">hello current required libraries are listed in hello [phonegap-plugin-push installation documentation][19].</span></span>

#### <a name="test-push-notifications-in-hello-app-on-android"></a><span data-ttu-id="78623-180">Teszt leküldéses értesítések Android rendszeren hello alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="78623-180">Test push notifications in hello app on Android</span></span>
<span data-ttu-id="78623-181">Most teszt leküldéses értesítések futtatásával hello alkalmazás és hello TodoItem tábla beszúrni elemeket is.</span><span class="sxs-lookup"><span data-stu-id="78623-181">You can now test push notifications by running hello app and inserting items in hello TodoItem table.</span></span> <span data-ttu-id="78623-182">Tesztelheti a hello ugyanarra az eszközre, illetve egy második eszközről mindaddig, amíg használ hello azonos háttér.</span><span class="sxs-lookup"><span data-stu-id="78623-182">You can test from hello same device or from a second device, as long as you are using hello same backend.</span></span> <span data-ttu-id="78623-183">Hello Android platformon a Cordova-alkalmazás tesztelése a következő módokon hello egyikében:</span><span class="sxs-lookup"><span data-stu-id="78623-183">Test your Cordova app on hello Android platform in one of hello following ways:</span></span>

* <span data-ttu-id="78623-184">**A fizikai eszközön:** a Android-eszköz tooyour fejlesztési számítógép USB-kábellel csatlakoztassa.</span><span class="sxs-lookup"><span data-stu-id="78623-184">**On a physical device:** Attach your Android device tooyour development computer with a USB cable.</span></span>  <span data-ttu-id="78623-185">Ahelyett, hogy **Google Android Emulator**, jelölje be **eszköz**.</span><span class="sxs-lookup"><span data-stu-id="78623-185">Instead of **Google Android Emulator**, select **Device**.</span></span> <span data-ttu-id="78623-186">Visual Studio hello alkalmazás toohello eszköz telepíti, és ezután a hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="78623-186">Visual Studio deploys hello application toohello device and then runs hello application.</span></span>  <span data-ttu-id="78623-187">Majd végezhet hello eszközön hello alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="78623-187">You can then interact with hello application on hello device.</span></span>

  <span data-ttu-id="78623-188">A fejlesztési felhasználói élmény javítása.</span><span class="sxs-lookup"><span data-stu-id="78623-188">Improve your development experience.</span></span>  <span data-ttu-id="78623-189">A képernyő megosztása például alkalmazások [Mobizen] [ 20] segíthet az Android alkalmazások fejlesztése.</span><span class="sxs-lookup"><span data-stu-id="78623-189">Screen sharing applications such as [Mobizen][20] can assist you in developing an Android application.</span></span>  <span data-ttu-id="78623-190">Mobizen projektek Android képernyő tooa webböngészőben a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="78623-190">Mobizen projects your Android screen tooa web browser on your PC.</span></span>

* <span data-ttu-id="78623-191">**Az Android-emulátorban:** az emulátor futtatásához szükséges további konfigurációs lépésekre.</span><span class="sxs-lookup"><span data-stu-id="78623-191">**On an Android emulator:** There are additional configuration steps required when running on an emulator.</span></span>

    <span data-ttu-id="78623-192">Ellenőrizze, hogy tooa virtuális eszköz, amely rendelkezik a Google API-k hello célként beállítása látható módon hello Android virtuális eszközt (AVD) manager telepít.</span><span class="sxs-lookup"><span data-stu-id="78623-192">Make sure you are deploying tooa virtual device that has Google APIs set as hello target, as shown in hello Android Virtual Device (AVD) manager.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    <span data-ttu-id="78623-193">Ha azt szeretné, toouse gyorsabb x86 emulátor, akkor [hello HAXM illesztőprogram telepítése] [ 11] és hello emulátor toouse konfigurálja azt.</span><span class="sxs-lookup"><span data-stu-id="78623-193">If you want toouse a faster x86 emulator, you [install hello HAXM driver][11] and configure hello emulator toouse it.</span></span>

    <span data-ttu-id="78623-194">Google fiók toohello Android-eszköz hozzáadásához kattintson **alkalmazások** > **beállítások** > **fiók hozzáadása**, majd kövesse a hello megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="78623-194">Add a Google account toohello Android device by clicking **Apps** > **Settings** > **Add account**, then follow hello prompts.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    <span data-ttu-id="78623-195">Futtassa a hello todolist alkalmazást előtt, és egy új teendő elem beszúrására.</span><span class="sxs-lookup"><span data-stu-id="78623-195">Run hello todolist app as before and insert a new todo item.</span></span> <span data-ttu-id="78623-196">Megadott idő hello értesítési területén egy értesítési ikon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="78623-196">This time, a notification icon is displayed in hello notification area.</span></span> <span data-ttu-id="78623-197">Hello értesítési fiók tooview hello teljes szöveges hello értesítés nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="78623-197">You can open hello notification drawer tooview hello full text of hello notification.</span></span>

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a><span data-ttu-id="78623-198">(Választható) Konfigurálására és futtatására IOS rendszerű eszközökön</span><span class="sxs-lookup"><span data-stu-id="78623-198">(Optional) Configure and run on iOS</span></span>
<span data-ttu-id="78623-199">Ez a szakasz az iOS-eszközök hello Cordova-projekt futtatása.</span><span class="sxs-lookup"><span data-stu-id="78623-199">This section is for running hello Cordova project on iOS devices.</span></span> <span data-ttu-id="78623-200">Ha nem dolgozik iOS-eszközökkel, ez a szakasz kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="78623-200">If you are not working with iOS devices, you can skip this section.</span></span>

#### <a name="install-and-run-hello-ios-remote-build-agent-on-a-mac-or-cloud-service"></a><span data-ttu-id="78623-201">Telepítés és távoli futási hello iOS ügynök létrehozása Mac vagy felhőalapú szolgáltatásként-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="78623-201">Install and run hello iOS remote build agent on a Mac or cloud service</span></span>
<span data-ttu-id="78623-202">A Cordova-alkalmazást a Visual Studio használatával iOS futtatása előtt végrehajtania hello lépéseket a hello [iOS beállítási útmutatója] [ 12] tooinstall és távoli futási hello készítsen ügynök.</span><span class="sxs-lookup"><span data-stu-id="78623-202">Before you can run a Cordova app on iOS using Visual Studio, go through hello steps in hello [iOS Setup Guide][12] tooinstall and run hello remote build agent.</span></span>

<span data-ttu-id="78623-203">Ellenőrizze, hogy hello alkalmazást IOS rendszerre hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="78623-203">Make sure you can build hello app for iOS.</span></span> <span data-ttu-id="78623-204">hello a hello beállítási útmutatója lépésekre az IOS rendszerhez készült Visual Studio szükséges toobuild.</span><span class="sxs-lookup"><span data-stu-id="78623-204">hello steps in hello setup guide are required toobuild for iOS from Visual Studio.</span></span> <span data-ttu-id="78623-205">Ha nem rendelkezik a Mac, IOS-egy szolgáltatás, például MacInCloud hello távoli build ügynök használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="78623-205">If you do not have a Mac, you can build for iOS using hello remote build agent on a service like MacInCloud.</span></span> <span data-ttu-id="78623-206">További információk: [az iOS-alkalmazás futtatása hello felhőben][21].</span><span class="sxs-lookup"><span data-stu-id="78623-206">For more info, see [Run your iOS app in hello cloud][21].</span></span>

> [!NOTE]
> <span data-ttu-id="78623-207">XCode 7-es vagy nagyobb szükség toouse hello leküldéses IOS beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="78623-207">XCode 7 or greater is required toouse hello push plugin on iOS.</span></span>

#### <a name="find-hello-id-toouse-as-your-app-id"></a><span data-ttu-id="78623-208">Az App-ID hello azonosító toouse keresése</span><span class="sxs-lookup"><span data-stu-id="78623-208">Find hello ID toouse as your App ID</span></span>
<span data-ttu-id="78623-209">Regisztrálja az alkalmazást a leküldéses értesítésekre, mielőtt config.xml nyissa meg a Cordova-alkalmazáshoz, a hello található `id` attribútumú hello widget elemben, és másolja azt a későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="78623-209">Before you register your app for push notifications, open config.xml in your Cordova app, find hello `id` attribute value in hello widget element, and copy it for later use.</span></span> <span data-ttu-id="78623-210">A következő XML hello, hello azonosító: `io.cordova.myapp7777777`.</span><span class="sxs-lookup"><span data-stu-id="78623-210">In hello following XML, hello ID is `io.cordova.myapp7777777`.</span></span>

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

<span data-ttu-id="78623-211">Ez az azonosító újabb verzióival való használatához, amikor egy alkalmazás azonosítója hoz létre az Apple fejlesztői portáljáról.</span><span class="sxs-lookup"><span data-stu-id="78623-211">Later, use this identifier when you create an App ID on Apple's developer portal.</span></span> <span data-ttu-id="78623-212">Ha hello fejlesztői portálján hoz létre egy másik alkalmazás Azonosítóját, akkor néhány további lépés szükséges, az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="78623-212">If you create a different App ID on hello developer portal, you must take a few extra steps later in this tutorial.</span></span> <span data-ttu-id="78623-213">a widget elem hello Azonosítójának meg kell egyeznie a hello Alkalmazásazonosító hello developer portálon.</span><span class="sxs-lookup"><span data-stu-id="78623-213">hello ID in the widget element must match hello App ID on hello developer portal.</span></span>

#### <a name="register-hello-app-for-push-notifications-on-apples-developer-portal"></a><span data-ttu-id="78623-214">Hello alkalmazás az Apple fejlesztői portálján a leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="78623-214">Register hello app for push notifications on Apple's developer portal</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[<span data-ttu-id="78623-215">Tekintse meg hasonló lépéseket ismertető videót</span><span class="sxs-lookup"><span data-stu-id="78623-215">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="78623-216">Az Azure toosend leküldéses értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78623-216">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a><span data-ttu-id="78623-217">Győződjön meg arról, hogy az alkalmazás azonosítója egyezik-e a Cordova-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="78623-217">Verify that your App ID matches your Cordova app</span></span>
<span data-ttu-id="78623-218">Ha már létrehozott az Apple fejlesztői fiókban Alkalmazásazonosító hello hello widget elemének config.xml hello azonosítója megegyezik, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="78623-218">If hello App ID you created in your Apple Developer Account already matches hello ID of hello widget element in config.xml, you can skip this step.</span></span> <span data-ttu-id="78623-219">Azonban ha hello azonosító nem egyezik, ideig hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="78623-219">However, if hello IDs don't match, take hello following steps:</span></span>

1. <span data-ttu-id="78623-220">A projekt hello platformok mappa törlése.</span><span class="sxs-lookup"><span data-stu-id="78623-220">Delete hello platforms folder from your project.</span></span>
2. <span data-ttu-id="78623-221">A projekt hello beépülő modulok mappa törlése.</span><span class="sxs-lookup"><span data-stu-id="78623-221">Delete hello plugins folder from your project.</span></span>
3. <span data-ttu-id="78623-222">A projekt hello node_modules mappa törlése.</span><span class="sxs-lookup"><span data-stu-id="78623-222">Delete hello node_modules folder from your project.</span></span>
4. <span data-ttu-id="78623-223">Frissítés hello az id attribútum hello widget elemének config.xml toouse hello alkalmazás azonosítója, amelyet az Apple fejlesztői fiókban hozott létre.</span><span class="sxs-lookup"><span data-stu-id="78623-223">Update hello id attribute of hello widget element in config.xml toouse hello App ID that you created in your  Apple Developer Account.</span></span>
5. <span data-ttu-id="78623-224">A projekt újraépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="78623-224">Rebuild your project.</span></span>

##### <a name="test-push-notifications-in-your-ios-app"></a><span data-ttu-id="78623-225">Teszt leküldéses értesítéseket az iOS-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="78623-225">Test push notifications in your iOS app</span></span>
1. <span data-ttu-id="78623-226">Visual Studio, ügyeljen arra, hogy **iOS** hello központi telepítés céljaként van kiválasztva, és válassza a **eszköz** toorun csatlakoztatott iOS-eszközön.</span><span class="sxs-lookup"><span data-stu-id="78623-226">In Visual Studio, make sure that **iOS** is selected as hello deployment target, and then choose **Device** toorun on your connected iOS device.</span></span>

    <span data-ttu-id="78623-227">Az iOS-eszköz csatlakoztatásakor tooyour PC futtathatja iTunes használatával.</span><span class="sxs-lookup"><span data-stu-id="78623-227">You can run on an iOS device connected tooyour PC using iTunes.</span></span> <span data-ttu-id="78623-228">hello iOS-szimulátor nem támogatja a leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="78623-228">hello iOS Simulator does not support push notifications.</span></span>

2. <span data-ttu-id="78623-229">Nyomja le az hello **futtatása** gomb vagy **F5** a Visual Studio toobuild hello projektet és a start hello alkalmazást az iOS-eszközt, majd kattintson **OK** tooaccept leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="78623-229">Press hello **Run** button or **F5** in Visual Studio toobuild hello project and start hello app in an iOS  device, then click **OK** tooaccept push notifications.</span></span>

   > [!NOTE]
   > <span data-ttu-id="78623-230">hello app során először futtassa hello leküldéses értesítések megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="78623-230">hello app requests confirmation for push notifications during hello first run.</span></span>

3. <span data-ttu-id="78623-231">Hello alkalmazásban írjon be egy feladatot, és kattintson a hello plusz (+) ikonra.</span><span class="sxs-lookup"><span data-stu-id="78623-231">In hello app, type a task, and then click hello plus (+) icon.</span></span>
4. <span data-ttu-id="78623-232">Győződjön meg arról, hogy értesítést kap, majd kattintson az OK toodismiss hello értesítést.</span><span class="sxs-lookup"><span data-stu-id="78623-232">Verify that a notification is received, then click OK toodismiss hello notification.</span></span>

## <a name="optional-configure-and-run-on-windows"></a><span data-ttu-id="78623-233">(Választható) Konfigurálja és futtassa a Windows</span><span class="sxs-lookup"><span data-stu-id="78623-233">(Optional) Configure and run on Windows</span></span>
<span data-ttu-id="78623-234">Ez a szakasz olyan hello Apache Cordova-alkalmazás projekt futó Windows 10 rendszerű eszközökhöz (Windows 10 hello PhoneGap leküldéses beépülő modul támogatja).</span><span class="sxs-lookup"><span data-stu-id="78623-234">This section is for running hello Apache Cordova app project on Windows 10 devices (hello PhoneGap push plugin is supported on Windows 10).</span></span> <span data-ttu-id="78623-235">Ha nem dolgozik Windows-eszközökkel, ez a szakasz kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="78623-235">If you are not working with Windows devices, you can skip this section.</span></span>

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a><span data-ttu-id="78623-236">Alkalmazás regisztrálása a Windows leküldéses értesítéseket a WNS-sel való</span><span class="sxs-lookup"><span data-stu-id="78623-236">Register your Windows app for push notifications with WNS</span></span>
<span data-ttu-id="78623-237">toouse hello tárolási lehetőségek Visual Studio, válasszon ki egy Windows cél listából hello megoldás platformok, például **Windows-x64** vagy **Windows-x86** (elkerülése érdekében **Windows-AnyCPU** a leküldéses értesítések).</span><span class="sxs-lookup"><span data-stu-id="78623-237">toouse hello Store options in Visual Studio, select a Windows target from hello Solution Platforms list, like **Windows-x64** or **Windows-x86** (avoid **Windows-AnyCPU** for push notifications).</span></span>

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

<span data-ttu-id="78623-238">[Egy hasonló lépéseket bemutató videó][13]</span><span class="sxs-lookup"><span data-stu-id="78623-238">[Watch a video showing similar steps][13]</span></span>

#### <a name="configure-hello-notification-hub-for-wns"></a><span data-ttu-id="78623-239">A WNS hello értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78623-239">Configure hello notification hub for WNS</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-toosupport-windows-push-notifications"></a><span data-ttu-id="78623-240">A Cordova-alkalmazás toosupport Windows leküldéses értesítések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="78623-240">Configure your Cordova app toosupport Windows push notifications</span></span>
<span data-ttu-id="78623-241">Nyissa meg hello configuration designer (kattintson a jobb gombbal a config.xml, és válassza **adatforrásnézet-tervezőből**), jelölje be hello **Windows** lapot, és válassza a **Windows 10** alatt**Windows verzió cél**.</span><span class="sxs-lookup"><span data-stu-id="78623-241">Open hello configuration designer (right-click on config.xml and select **View Designer**), select hello **Windows** tab, and choose **Windows 10** under **Windows Target Version**.</span></span>

<span data-ttu-id="78623-242">toosupport leküldéses értesítések az alapértelmezett (hibakereséshez) hoz létre, nyissa meg build.json fájl.</span><span class="sxs-lookup"><span data-stu-id="78623-242">toosupport push notifications in your default (debug) builds, open build.json file.</span></span> <span data-ttu-id="78623-243">Másolja a "release" konfigurációs tooyour hibakeresési beállításait.</span><span class="sxs-lookup"><span data-stu-id="78623-243">Copy the "release" configuration tooyour debug configuration.</span></span>

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

<span data-ttu-id="78623-244">Hello frissítés után hello build.json tartalmaznia kell a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="78623-244">After hello update, hello build.json should contain hello following code:</span></span>

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

<span data-ttu-id="78623-245">Hozza létre hello alkalmazását, és ellenőrizze, hogy rendelkezik-e hibák.</span><span class="sxs-lookup"><span data-stu-id="78623-245">Build hello app and verify that you have no errors.</span></span> <span data-ttu-id="78623-246">Az ügyfélalkalmazás most hello Mobile Apps-háttéralkalmazás hello értesítéseihez kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="78623-246">Your client app should now register for hello notifications from hello Mobile App backend.</span></span> <span data-ttu-id="78623-247">Ismételje meg minden Windows-projektet a megoldásban ez a szakasz.</span><span class="sxs-lookup"><span data-stu-id="78623-247">Repeat this section for every Windows project in your solution.</span></span>

#### <a name="test-push-notifications-in-your-windows-app"></a><span data-ttu-id="78623-248">Teszt leküldéses értesítések a Windows-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="78623-248">Test push notifications in your Windows app</span></span>
<span data-ttu-id="78623-249">A Visual Studio, győződjön meg arról, hogy a Windows platform van kiválasztva a telepítés céljaként hello, például a **Windows-x64** vagy **Windows-x86**.</span><span class="sxs-lookup"><span data-stu-id="78623-249">In Visual Studio, make sure that a Windows platform is selected as hello deployment target, such as **Windows-x64** or **Windows-x86**.</span></span> <span data-ttu-id="78623-250">Válassza ki a Visual Studio üzemeltető Windows 10 rendszerű toorun hello app **helyi számítógép**.</span><span class="sxs-lookup"><span data-stu-id="78623-250">toorun hello app on a Windows 10 PC hosting Visual Studio, choose **Local Machine**.</span></span>

<span data-ttu-id="78623-251">Nyomja le az hello futtassa gomb toobuild hello projektet, és indítsa el hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="78623-251">Press hello Run button toobuild hello project and start hello app.</span></span>

<span data-ttu-id="78623-252">Hello alkalmazásban, írja be egy új todoitem nevét, és kattintson a hello plusz (+) ikonra tooadd azt.</span><span class="sxs-lookup"><span data-stu-id="78623-252">In hello app, type a name for a new todoitem, and then click hello plus (+) icon tooadd it.</span></span>

<span data-ttu-id="78623-253">Győződjön meg arról, hogy értesítést kapott, hello cikk felvételekor.</span><span class="sxs-lookup"><span data-stu-id="78623-253">Verify that a notification is received when hello item is added.</span></span>

## <span data-ttu-id="78623-254"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78623-254"><a name="next-steps"></a>Next Steps</span></span>
* <span data-ttu-id="78623-255">További információ a [Notification Hubs] [ 17] toolearn kapcsolatos leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="78623-255">Read about [Notification Hubs][17] toolearn about push notifications.</span></span>
* <span data-ttu-id="78623-256">Ha még nem tette meg, továbbra is hello oktatóanyag által [hozzáadása hitelesítési] [ 14] tooyour Apache Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="78623-256">If you have not already done so, continue hello tutorial by [Adding Authentication][14] tooyour Apache Cordova app.</span></span>

<span data-ttu-id="78623-257">Ismerje meg, hogyan toouse hello SDK-k.</span><span class="sxs-lookup"><span data-stu-id="78623-257">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="78623-258">[Apache Cordova-SDK][15]</span><span class="sxs-lookup"><span data-stu-id="78623-258">[Apache Cordova SDK][15]</span></span>
* <span data-ttu-id="78623-259">[ASP.NET Server SDK][1]</span><span class="sxs-lookup"><span data-stu-id="78623-259">[ASP.NET Server SDK][1]</span></span>
* <span data-ttu-id="78623-260">[NODE.js Server SDK][16]</span><span class="sxs-lookup"><span data-stu-id="78623-260">[Node.js Server SDK][16]</span></span>

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: http://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: http://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: http://taco.visualstudio.com/en-us/docs/build_ios_cloud/
