---
title: "az Azure Mobile Engagement a Cordova/Phonegap elindítva aaaGet"
description: "Megtudhatja, hogyan Cordova/Phonegap-alkalmazásokhoz az Azure Mobile Engagement az elemzések és leküldéses értesítések toouse."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 54fe9113-e239-4ed7-9fd1-a502d7ac7f47
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-phonegap
ms.devlang: js
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: e67dabbdf7886802bb058f38964e558d5ae6854c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a><span data-ttu-id="8fcef-103">Ismerkedés az Azure Mobile Engagement Cordova/Phonegap-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="8fcef-103">Get Started with Azure Mobile Engagement for Cordova/Phonegap</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="8fcef-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand a használati és küldési leküldéses értesítések toosegmented felhasználók egy mobilalkalmazás Cordova használatával fejlesztett.</span><span class="sxs-lookup"><span data-stu-id="8fcef-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users for a mobile application developed with Cordova.</span></span>

<span data-ttu-id="8fcef-105">Ebben az oktatóanyagban létre fogunk hozni egy üres Cordova-alkalmazást Mac használatával, majd integráljuk a Mobile Engagement SDK-t.</span><span class="sxs-lookup"><span data-stu-id="8fcef-105">In this tutorial, we will create a blank Cordova app using Mac and then integrate Mobile Engagement SDK.</span></span> <span data-ttu-id="8fcef-106">Az alkalmazás alapszintű elemzési adatokat gyűjt össze, és leküldéses értesítéseket fogad az iOS rendszerhez készült Apple leküldéses értesítési rendszerének (APNS) és az Android Google Cloud Messaging (GCM) szolgáltatásának a használatával.</span><span class="sxs-lookup"><span data-stu-id="8fcef-106">It collects basic analytics data and receives push notifications using Apple Push Notification System (APNS) for iOS and Google Cloud Messaging (GCM) for Android.</span></span> <span data-ttu-id="8fcef-107">Fogjuk a tooan iOS vagy Android-eszköz tesztelési üzembe helyezni.</span><span class="sxs-lookup"><span data-stu-id="8fcef-107">We will deploy this tooan iOS or Android device for testing.</span></span> 

> [!NOTE]
> <span data-ttu-id="8fcef-108">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="8fcef-108">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="8fcef-109">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="8fcef-109">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="8fcef-110">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).</span><span class="sxs-lookup"><span data-stu-id="8fcef-110">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).</span></span>
> 
> 

<span data-ttu-id="8fcef-111">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="8fcef-111">This tutorial requires hello following:</span></span>

* <span data-ttu-id="8fcef-112">XCode, amely telepítése Mac App Store-ból (üzembe helyezéshez tooiOS)</span><span class="sxs-lookup"><span data-stu-id="8fcef-112">XCode, which you can install from Mac App Store (for deploying tooiOS)</span></span>
* <span data-ttu-id="8fcef-113">[Android SDK és -emulátor](http://developer.android.com/sdk/installing/index.html) (üzembe helyezéshez tooAndroid)</span><span class="sxs-lookup"><span data-stu-id="8fcef-113">[Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (for deploying tooAndroid)</span></span>
* <span data-ttu-id="8fcef-114">leküldéses értesítési tanúsítvány (.p12), amelyet az Apple APNS fejlesztési központjában szerezhet be,</span><span class="sxs-lookup"><span data-stu-id="8fcef-114">Push notification certificate (.p12) that you can obtain from Apple Dev Center for APNS</span></span>
* <span data-ttu-id="8fcef-115">GCM-projektszám, amelyet a Google Developer Console for GCM konzolon keresztül szerezhet be,</span><span class="sxs-lookup"><span data-stu-id="8fcef-115">GCM Project number that you can obtain from your Google Developer Console for GCM</span></span>
* [<span data-ttu-id="8fcef-116">Mobile Engagement Cordova beépülő modul</span><span class="sxs-lookup"><span data-stu-id="8fcef-116">Mobile Engagement Cordova Plugin</span></span>](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [!NOTE]
> <span data-ttu-id="8fcef-117">Hello forráskód kereshet és információs hello hello Cordova beépülő modul a [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova)</span><span class="sxs-lookup"><span data-stu-id="8fcef-117">You can find hello source code and hello ReadMe for hello Cordova plugin on [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova)</span></span>
> 
> 

## <span data-ttu-id="8fcef-118"><a id="setup-azme"></a>A Mobile Engagement beállítása a Cordova-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="8fcef-118"><a id="setup-azme"></a>Setup Mobile Engagement for your Cordova app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="8fcef-119"><a id="connecting-app"></a>Kapcsolódás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="8fcef-119"><a id="connecting-app"></a>Connecting your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="8fcef-120">Ez az oktatóanyag egy "alapszintű integrációt" mutat minimális hello határozza meg a szükséges toocollect dátumát és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="8fcef-120">This tutorial presents a "basic integration" which is hello minimal set required toocollect data and send a push notification.</span></span> 

<span data-ttu-id="8fcef-121">Létre fogunk hozni egy alapszintű alkalmazást a Cordova toodemonstrate hello integrációja:</span><span class="sxs-lookup"><span data-stu-id="8fcef-121">We will create a basic app with Cordova toodemonstrate hello integration:</span></span>

### <a name="create-a-new-cordova-project"></a><span data-ttu-id="8fcef-122">Új Cordova-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="8fcef-122">Create a new Cordova project</span></span>
1. <span data-ttu-id="8fcef-123">Indítsa el *Terminálszolgáltatások* ablakot a Mac számítógép és a típus hello követő, amely létrehoz egy új Cordova-projekt hello alapértelmezett sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="8fcef-123">Launch *Terminal* window on your Mac machine and type hello following which will create a new Cordova project from hello default template.</span></span> <span data-ttu-id="8fcef-124">Győződjön meg arról, hogy hello közzétételi profilt, végül használata toodeploy az iOS-alkalmazás által használt "com.mycompany.myapp", mert hello alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="8fcef-124">Make sure that hello publishing profile you eventually use toodeploy your iOS app is using 'com.mycompany.myapp' as hello App ID.</span></span> 
   
        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova
2. <span data-ttu-id="8fcef-125">A projekt hajtható végre a következő tooconfigure hello **iOS** és futtassa azt hello iOS-szimulátor:</span><span class="sxs-lookup"><span data-stu-id="8fcef-125">Execute hello following tooconfigure your project for **iOS** and run it in hello iOS Simulator:</span></span>
   
        $ cordova platform add ios 
        $ cordova run ios
3. <span data-ttu-id="8fcef-126">A projekt hajtható végre a következő tooconfigure hello **Android** és hello Android-emulátorban való futtatáshoz.</span><span class="sxs-lookup"><span data-stu-id="8fcef-126">Execute hello following tooconfigure your project for **Android** and run it in hello Android emulator.</span></span> <span data-ttu-id="8fcef-127">Győződjön meg arról, hogy az Android SDK Emulator beállításainál rendelkezik a cél Google APIs (Google Inc.) hello CPU / ABI értéke pedig a Google APIs ARM.</span><span class="sxs-lookup"><span data-stu-id="8fcef-127">Make sure that your Android SDK Emulator settings have its Target as Google APIs (Google Inc.) with hello CPU / ABI as Google APIs ARM.</span></span>  
   
        $ cordova platform add android
        $ cordova run android
4. <span data-ttu-id="8fcef-128">Adja hozzá a Cordova-konzol beépülő modul hello.</span><span class="sxs-lookup"><span data-stu-id="8fcef-128">Add hello Cordova Console plugin.</span></span> 

    ```
    $ cordova plugin add cordova-plugin-console
    ``` 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="8fcef-129">Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="8fcef-129">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="8fcef-130">Hello Azure Mobile Engagement Cordova beépülő modul telepítése ugyanakkor biztosítható a hello változók értékeinek tooconfigure hello beépülő modult:</span><span class="sxs-lookup"><span data-stu-id="8fcef-130">Install hello Azure Mobile Engagement Cordova plugin while providing hello variable values tooconfigure hello plugin:</span></span>
   
        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
             --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers hello app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

<span data-ttu-id="8fcef-131">*Android Reach Icon* : hello bármely kiterjesztést, vagy a rajzolható előtag nélkül hello erőforrás nevének kell lennie (például: mynotificationicon), és hello ikonfájlt az android-projekt (platformok/android/res/drawable) át kell másolni</span><span class="sxs-lookup"><span data-stu-id="8fcef-131">*Android Reach Icon* : must be hello name of hello resource without any extension, nor drawable prefix (ex: mynotificationicon), and hello icon file must be copied into your android project (platforms/android/res/drawable)</span></span>

<span data-ttu-id="8fcef-132">*iOS Reach Icon* : hello a kiterjesztéssel együtt hello erőforrás nevének kell lennie (például: mynotificationicon.png), és hello ikonfájlt hozzá kell adni az iOS-projekthez az XCode (hello Hozzáadás fájlok menü használatával)</span><span class="sxs-lookup"><span data-stu-id="8fcef-132">*iOS Reach Icon*  : must be hello name of hello resource with its extension (ex:  mynotificationicon.png), and hello icon file must be added into your iOS project with XCode (using hello Add Files Menu)</span></span>

## <span data-ttu-id="8fcef-133"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8fcef-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
1. <span data-ttu-id="8fcef-134">A Cordova-projekt hello - szerkesztése **www/js/index.js** tooadd hello hívás tooMobile Engagement toodeclare új tevékenységet, egyszer hello *deviceReady* esemény érkezik.</span><span class="sxs-lookup"><span data-stu-id="8fcef-134">In hello Cordova project - edit **www/js/index.js** tooadd hello call tooMobile Engagement toodeclare a new activity once hello *deviceReady* event is received.</span></span>
   
         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }
2. <span data-ttu-id="8fcef-135">Hello alkalmazás futtatásához:</span><span class="sxs-lookup"><span data-stu-id="8fcef-135">Run hello application:</span></span>
   
   * <span data-ttu-id="8fcef-136">**iOS esetén**</span><span class="sxs-lookup"><span data-stu-id="8fcef-136">**For iOS**</span></span>
     
       <span data-ttu-id="8fcef-137">A `Terminal` ablakban indítsa el az alkalmazást a Simulator új példányában hello alábbiak végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="8fcef-137">In `Terminal` window launch your app in a new Simulator instance by executing hello following:</span></span>
     
           cordova run ios
   * <span data-ttu-id="8fcef-138">**Android esetén**</span><span class="sxs-lookup"><span data-stu-id="8fcef-138">**For Android**</span></span>
     
       <span data-ttu-id="8fcef-139">A `Terminal` ablakban indítsa el az alkalmazást az emulátor új példányában hello alábbiak végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="8fcef-139">In `Terminal` window launch your app in a new emulator instance by executing hello following:</span></span>
     
           cordova run android
3. <span data-ttu-id="8fcef-140">Hello konzolnaplófájlokban hello következő látható:</span><span class="sxs-lookup"><span data-stu-id="8fcef-140">You can see hello following in hello console logs:</span></span>
   
        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

## <span data-ttu-id="8fcef-141"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="8fcef-141"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="8fcef-142"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8fcef-142"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="8fcef-143">Mobile Engagement lehetővé teszi toointeract a felhasználókkal leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel hello kampányok.</span><span class="sxs-lookup"><span data-stu-id="8fcef-143">Mobile Engagement allows you toointeract with your users using Push Notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="8fcef-144">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="8fcef-144">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="8fcef-145">hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="8fcef-145">hello following sections will setup your app tooreceive them.</span></span>

### <a name="configure-push-credentials-for-mobile-engagement"></a><span data-ttu-id="8fcef-146">Leküldési hitelesítő adatok konfigurálása a Mobile Engagementhez</span><span class="sxs-lookup"><span data-stu-id="8fcef-146">Configure Push credentials for Mobile Engagement</span></span>
<span data-ttu-id="8fcef-147">tooallow a Mobile Engagement toosend leküldéses értesítések küldése az Ön nevében, meg kell toogrant azt elérni tooyour Apple iOS-tanúsítvány vagy a GCM kiszolgálói API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="8fcef-147">tooallow Mobile Engagement toosend Push Notifications on your behalf, you need toogrant it access tooyour Apple iOS certificate or GCM Server API Key.</span></span> 

1. <span data-ttu-id="8fcef-148">Keresse meg a tooyour Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="8fcef-148">Navigate tooyour Mobile Engagement portal.</span></span> <span data-ttu-id="8fcef-149">Nyissa meg azt a projekt használ, és kattintson a hello hello alkalmazásban **Engage** hello alsó gombra:</span><span class="sxs-lookup"><span data-stu-id="8fcef-149">Ensure you're in hello app we're using for this project and then click on hello **Engage** button at hello bottom:</span></span>
   
    ![][1]
2. <span data-ttu-id="8fcef-150">Ekkor megnyílik a hello beállítások lapra az Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="8fcef-150">You will land in hello settings page in your Engagement Portal.</span></span> <span data-ttu-id="8fcef-151">Kattintson a hello **natív leküldés** szakasz:</span><span class="sxs-lookup"><span data-stu-id="8fcef-151">From there click on hello **Native Push** section:</span></span>
   
    ![][2]
3. <span data-ttu-id="8fcef-152">iOS-tanúsítvány/GCM-kiszolgáló API-kulcsának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8fcef-152">Configure iOS Certificate/GCM Server API Key</span></span>
   
    <span data-ttu-id="8fcef-153">**iOS**</span><span class="sxs-lookup"><span data-stu-id="8fcef-153">**[iOS]**</span></span>
   
    <span data-ttu-id="8fcef-154">a.</span><span class="sxs-lookup"><span data-stu-id="8fcef-154">a.</span></span> <span data-ttu-id="8fcef-155">Jelölje ki .p12 tanúsítványát, töltse fel, és írja be jelszavát:</span><span class="sxs-lookup"><span data-stu-id="8fcef-155">Select your .p12, upload it and type your password:</span></span>
   
    ![][3]
   
    <span data-ttu-id="8fcef-156">**Android**</span><span class="sxs-lookup"><span data-stu-id="8fcef-156">**[Android]**</span></span>
   
    <span data-ttu-id="8fcef-157">a.</span><span class="sxs-lookup"><span data-stu-id="8fcef-157">a.</span></span> <span data-ttu-id="8fcef-158">Hello Szerkesztés ikonra elé **API-kulcs** hello GCM Settings szakasz és hello előugró ablak, amely mutatja be, illessze be a hello GCM kiszolgálói kulcsot, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="8fcef-158">Click hello edit icon in front of **API Key** in hello GCM Settings section and in hello popup which shows up, paste hello GCM Server Key and click **OK**.</span></span> 
   
    ![][4]

### <a name="enable-push-notifications-in-hello-cordova-app"></a><span data-ttu-id="8fcef-159">Hello Cordova-alkalmazás leküldési értesítések engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8fcef-159">Enable push notifications in hello Cordova app</span></span>
<span data-ttu-id="8fcef-160">Szerkesztés **www/js/index.js** tooadd hello hívás tooMobile Engagement toorequest leküldéses értesítések és egy kezelő deklarálásához:</span><span class="sxs-lookup"><span data-stu-id="8fcef-160">Edit **www/js/index.js** tooadd hello call tooMobile Engagement toorequest push notifications and declare a handler:</span></span>

     onDeviceReady: function() {
           Engagement.initializeReach(  
                 // on OpenUrl  
                 function(_url) {   
                 alert(_url);   
                 });  
            Engagement.startActivity("myPage",{});  
        }

### <a name="run-hello-app"></a><span data-ttu-id="8fcef-161">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="8fcef-161">Run hello app</span></span>
<span data-ttu-id="8fcef-162">**iOS**</span><span class="sxs-lookup"><span data-stu-id="8fcef-162">**[iOS]**</span></span>

1. <span data-ttu-id="8fcef-163">Rendszer XCode toobuild használja és hello eszköz tootest leküldéses értesítések hello alkalmazás telepítése, mivel az iOS csak lehetővé teszi a leküldéses értesítések tooan tényleges eszközön.</span><span class="sxs-lookup"><span data-stu-id="8fcef-163">We will use XCode toobuild and deploy hello app on hello device tootest push notifications since iOS only allows push notifications tooan actual device.</span></span> <span data-ttu-id="8fcef-164">Nyissa meg a Cordova-projekt létrehozási helyének toohello helyét, és keresse meg a túl**...\platforms\ios** helyét.</span><span class="sxs-lookup"><span data-stu-id="8fcef-164">Go toohello location where your Cordova project is created and navigate too**...\platforms\ios** location.</span></span> <span data-ttu-id="8fcef-165">Nyissa meg hello natív .xcodeproj fájlt az xcode-ban.</span><span class="sxs-lookup"><span data-stu-id="8fcef-165">Open up hello native .xcodeproj file in XCode.</span></span> 
2. <span data-ttu-id="8fcef-166">Hozza létre és hello Cordova app toohello iOS-eszközt hello fiókot, amely rendelkezik hello létesítési profilt, amely tartalmazza az imént feltöltött szolgáltatáscsomagban toohello Mobile Engagement portálra és hello alkalmazásazonosító, amely megfelel egy létrehozásakor megadott hello hello tanúsítvány telepítése hello Cordova-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8fcef-166">Build and deploy hello Cordova app toohello iOS device using hello account which has hello provisioning profile containing hello certificate you just uploaded toohello Mobile Engagement portal and hello App Id which matches hello one you provided while creating hello Cordova app.</span></span> <span data-ttu-id="8fcef-167">Hello megtekintheti *csomagazonosítót* a a **erőforrások\*-info.plist** fájlt az XCode toomatch azt be.</span><span class="sxs-lookup"><span data-stu-id="8fcef-167">You can check out hello *Bundle identifier* in your **Resources\*-info.plist** file in XCode toomatch it up.</span></span> 
3. <span data-ttu-id="8fcef-168">Hello szokásos iOS előugró ablak, az eszközön hello alkalmazást kér engedélyt toosend értesítések jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8fcef-168">You will see hello standard iOS popup on your device saying that hello app requests permission toosend notifications.</span></span> <span data-ttu-id="8fcef-169">Hello engedélyt.</span><span class="sxs-lookup"><span data-stu-id="8fcef-169">Grant hello permission.</span></span> 

<span data-ttu-id="8fcef-170">**Android**</span><span class="sxs-lookup"><span data-stu-id="8fcef-170">**[Android]**</span></span>

<span data-ttu-id="8fcef-171">Segítségével egyszerűen hello emulátor toorun hello Android-alkalmazás GCM értesítések támogatottak a hello Android-emulátorban.</span><span class="sxs-lookup"><span data-stu-id="8fcef-171">You can simply use hello emulator toorun hello Android app as GCM notifications are supported on hello Android emulator.</span></span> 

    cordova run android

## <span data-ttu-id="8fcef-172"><a id="send"></a>Egy értesítési tooyour app küldése</span><span class="sxs-lookup"><span data-stu-id="8fcef-172"><a id="send"></a>Send a notification tooyour app</span></span>
<span data-ttu-id="8fcef-173">Most létrehozunk egy egyszerű leküldéses értesítési kampányt, amely elküld egy leküldéses-tooyour hello eszközön futó alkalmazáshoz:</span><span class="sxs-lookup"><span data-stu-id="8fcef-173">We will now create a simple Push Notification campaign that will send a push tooyour app running on hello device:</span></span>

1. <span data-ttu-id="8fcef-174">Keresse meg a toohello **elérni** a Mobile Engagement portálra a lap</span><span class="sxs-lookup"><span data-stu-id="8fcef-174">Navigate toohello **Reach** tab in your Mobile Engagement portal</span></span>
2. <span data-ttu-id="8fcef-175">Kattintson a **új hirdetmény** toocreate a leküldéses kampány</span><span class="sxs-lookup"><span data-stu-id="8fcef-175">Click **New Announcement** toocreate your push campaign</span></span>
   
    ![][6]
3. <span data-ttu-id="8fcef-176">Adja meg a bemeneti adatok toocreate a kampány **[Android]**</span><span class="sxs-lookup"><span data-stu-id="8fcef-176">Provide inputs toocreate your campaign **[Android]**</span></span>
   
   * <span data-ttu-id="8fcef-177">Adja meg a kampány nevét a **Name** mezőben.</span><span class="sxs-lookup"><span data-stu-id="8fcef-177">Provide a **Name** for your campaign.</span></span> 
   * <span data-ttu-id="8fcef-178">Jelölje be hello **kézbesítési típust** , *Rendszerértesítő* *egyszerű*</span><span class="sxs-lookup"><span data-stu-id="8fcef-178">Select hello **Delivery Type** as *System notification* *Simple*</span></span>
   * <span data-ttu-id="8fcef-179">Jelölje be hello **kézbesítési időhöz** , *"Minden alkalommal"*</span><span class="sxs-lookup"><span data-stu-id="8fcef-179">Select hello **Delivery time** as *"Any Time"*</span></span>
   * <span data-ttu-id="8fcef-180">Adjon meg egy **cím** a Ez az első sor hello hello leküldéses értesítést.</span><span class="sxs-lookup"><span data-stu-id="8fcef-180">Provide a **Title** for your notification which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="8fcef-181">Adjon meg egy **üzenet** az értesítéshez, amely hello üzenettörzs erre a célra.</span><span class="sxs-lookup"><span data-stu-id="8fcef-181">Provide a **Message** for your notification which will serve as hello message body.</span></span> 
     
     ![][11]
4. <span data-ttu-id="8fcef-182">Adja meg a bemeneti adatok toocreate a kampány **[iOS]**</span><span class="sxs-lookup"><span data-stu-id="8fcef-182">Provide inputs toocreate your campaign **[iOS]**</span></span>
   
   * <span data-ttu-id="8fcef-183">Adja meg a kampány nevét a **Name** mezőben.</span><span class="sxs-lookup"><span data-stu-id="8fcef-183">Provide a **Name** for your campaign.</span></span> 
   * <span data-ttu-id="8fcef-184">Jelölje be hello **kézbesítési időhöz** , *"csak az alkalmazáson kívül"*</span><span class="sxs-lookup"><span data-stu-id="8fcef-184">Select hello **Delivery time** as *"Out of app only"*</span></span>
   * <span data-ttu-id="8fcef-185">Adjon meg egy **cím** a Ez az első sor hello hello leküldéses értesítést.</span><span class="sxs-lookup"><span data-stu-id="8fcef-185">Provide a **Title** for your notification which will be hello first line in hello push.</span></span>
   * <span data-ttu-id="8fcef-186">Adjon meg egy **üzenet** az értesítéshez, amely hello üzenettörzs erre a célra.</span><span class="sxs-lookup"><span data-stu-id="8fcef-186">Provide a **Message** for your notification which will serve as hello message body.</span></span> 
     
     ![][12]
5. <span data-ttu-id="8fcef-187">Görgessen lefelé, és a hello tartalomszakasz válassza **csak értesítés**</span><span class="sxs-lookup"><span data-stu-id="8fcef-187">Scroll down, and in hello content section select **Notification only**</span></span>
   
    ![][8]
6. <span data-ttu-id="8fcef-188">(Választható) Megadhatja egy művelet URL-címét is.</span><span class="sxs-lookup"><span data-stu-id="8fcef-188">[Optional] You can also provide an Action URL.</span></span> <span data-ttu-id="8fcef-189">Győződjön meg arról, hogy használja-e a hello beépülő modul konfigurálása során megadott URL-sémát **AZME\_ÁTIRÁNYÍTÁSI\_URL-cím** változó pl. *myapp://test*.</span><span class="sxs-lookup"><span data-stu-id="8fcef-189">Make sure that it uses a URL scheme provided while configuring hello plugin's **AZME\_REDIRECT\_URL** variable e.g. *myapp://test*.</span></span>  
7. <span data-ttu-id="8fcef-190">Elkészült a beállítással hello lehető legegyszerűbb kampányt lehetséges.</span><span class="sxs-lookup"><span data-stu-id="8fcef-190">You're done setting hello most basic campaign possible.</span></span> <span data-ttu-id="8fcef-191">Görgessen lefelé, és kattintson a hello **létrehozása** toosave gombra a kampány.</span><span class="sxs-lookup"><span data-stu-id="8fcef-191">Now scroll down again and click hello **Create** button toosave your campaign.</span></span>
8. <span data-ttu-id="8fcef-192">Végül aktiválja a kampányt az **Activate** (Aktiválás) gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="8fcef-192">Finally **Activate** your campaign</span></span>
   
    ![][10]
9. <span data-ttu-id="8fcef-193">Ekkor meg kellene jelennie egy leküldéses értesítésnek az eszközön vagy az emulátorban a jelen kampány részeként.</span><span class="sxs-lookup"><span data-stu-id="8fcef-193">You should now see a push notification on your device or emulator as part of this campaign.</span></span> 

## <span data-ttu-id="8fcef-194"><a id="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8fcef-194"><a id="next-steps"></a>Next Steps</span></span>
[<span data-ttu-id="8fcef-195">A Cordova Mobile Engagement SDK-val elérhető összes módszer áttekintése</span><span class="sxs-lookup"><span data-stu-id="8fcef-195">Overview of all methods available with Cordova Mobile Engagement SDK</span></span>](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

