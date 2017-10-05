---
title: "Ismerkedés az Azure Mobile Engagement Cordova/Phonegap-alkalmazásokkal való használatával"
description: "Ismerje meg, hogyan használható az Azure Mobile Engagement a Cordova/Phonegap-alkalmazásokhoz kapcsolódó elemzések és leküldéses értesítések tekintetében."
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
ms.openlocfilehash: d7a761310782faab1dda023785f93cf90742e2ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-cordovaphonegap"></a><span data-ttu-id="b15c9-103">Ismerkedés az Azure Mobile Engagement Cordova/Phonegap-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="b15c9-103">Get Started with Azure Mobile Engagement for Cordova/Phonegap</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="b15c9-104">Ebben a témakörben elsajátíthatja, hogy miként használható az Azure Mobile Engagement az alkalmazás használatának megértéséhez, valamint leküldéses értesítéseknek a Cordova használatával fejlesztett mobilalkalmazásba történő küldéséhez a szegmentált felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="b15c9-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users for a mobile application developed with Cordova.</span></span>

<span data-ttu-id="b15c9-105">Ebben az oktatóanyagban létre fogunk hozni egy üres Cordova-alkalmazást Mac használatával, majd integráljuk a Mobile Engagement SDK-t.</span><span class="sxs-lookup"><span data-stu-id="b15c9-105">In this tutorial, we will create a blank Cordova app using Mac and then integrate Mobile Engagement SDK.</span></span> <span data-ttu-id="b15c9-106">Az alkalmazás alapszintű elemzési adatokat gyűjt össze, és leküldéses értesítéseket fogad az iOS rendszerhez készült Apple leküldéses értesítési rendszerének (APNS) és az Android Google Cloud Messaging (GCM) szolgáltatásának a használatával.</span><span class="sxs-lookup"><span data-stu-id="b15c9-106">It collects basic analytics data and receives push notifications using Apple Push Notification System (APNS) for iOS and Google Cloud Messaging (GCM) for Android.</span></span> <span data-ttu-id="b15c9-107">Az alkalmazást iOS- vagy Android-eszközön fogjuk üzembe helyezni tesztelés céljából.</span><span class="sxs-lookup"><span data-stu-id="b15c9-107">We will deploy this to an iOS or Android device for testing.</span></span> 

> [!NOTE]
> <span data-ttu-id="b15c9-108">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="b15c9-108">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="b15c9-109">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="b15c9-109">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b15c9-110">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).</span><span class="sxs-lookup"><span data-stu-id="b15c9-110">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).</span></span>
> 
> 

<span data-ttu-id="b15c9-111">Az oktatóanyaghoz az alábbiakra lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="b15c9-111">This tutorial requires the following:</span></span>

* <span data-ttu-id="b15c9-112">XCode, amely a MAC App Store áruházából telepíthető (iOS rendszeren történő üzembe helyezéshez),</span><span class="sxs-lookup"><span data-stu-id="b15c9-112">XCode, which you can install from Mac App Store (for deploying to iOS)</span></span>
* <span data-ttu-id="b15c9-113">[Android SDK és -emulátor](http://developer.android.com/sdk/installing/index.html) (Android rendszeren történő üzembe helyezéshez),</span><span class="sxs-lookup"><span data-stu-id="b15c9-113">[Android SDK & Emulator](http://developer.android.com/sdk/installing/index.html) (for deploying to Android)</span></span>
* <span data-ttu-id="b15c9-114">leküldéses értesítési tanúsítvány (.p12), amelyet az Apple APNS fejlesztési központjában szerezhet be,</span><span class="sxs-lookup"><span data-stu-id="b15c9-114">Push notification certificate (.p12) that you can obtain from Apple Dev Center for APNS</span></span>
* <span data-ttu-id="b15c9-115">GCM-projektszám, amelyet a Google Developer Console for GCM konzolon keresztül szerezhet be,</span><span class="sxs-lookup"><span data-stu-id="b15c9-115">GCM Project number that you can obtain from your Google Developer Console for GCM</span></span>
* [<span data-ttu-id="b15c9-116">Mobile Engagement Cordova beépülő modul</span><span class="sxs-lookup"><span data-stu-id="b15c9-116">Mobile Engagement Cordova Plugin</span></span>](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [!NOTE]
> <span data-ttu-id="b15c9-117">A Cordova beépülő modul forráskódját és a ReadMe fájlt a [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova) webhelyén érheti el.</span><span class="sxs-lookup"><span data-stu-id="b15c9-117">You can find the source code and the ReadMe for the Cordova plugin on [GitHub](https://github.com/Azure/azure-mobile-engagement-cordova)</span></span>
> 
> 

## <span data-ttu-id="b15c9-118"><a id="setup-azme"></a>A Mobile Engagement beállítása a Cordova-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="b15c9-118"><a id="setup-azme"></a>Setup Mobile Engagement for your Cordova app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="b15c9-119"><a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="b15c9-119"><a id="connecting-app"></a>Connecting your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="b15c9-120">Ez az oktatóanyag egy „alapszintű integrációt” mutat be, ami minimálisan szükséges az adatok gyűjtéséhez és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="b15c9-120">This tutorial presents a "basic integration" which is the minimal set required to collect data and send a push notification.</span></span> 

<span data-ttu-id="b15c9-121">Létre fogunk hozni egy alapszintű alkalmazást a Cordova segítségével az integráció bemutatásához:</span><span class="sxs-lookup"><span data-stu-id="b15c9-121">We will create a basic app with Cordova to demonstrate the integration:</span></span>

### <a name="create-a-new-cordova-project"></a><span data-ttu-id="b15c9-122">Új Cordova-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b15c9-122">Create a new Cordova project</span></span>
1. <span data-ttu-id="b15c9-123">Indítsa el a *Terminál* ablakot a Mac számítógépen, és írja be az alábbiakat, amellyel létrehoz egy új Cordova-projektet az alapértelmezett sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="b15c9-123">Launch *Terminal* window on your Mac machine and type the following which will create a new Cordova project from the default template.</span></span> <span data-ttu-id="b15c9-124">Győződjön meg róla, hogy az iOS-alkalmazás majdani üzembe helyezéséhez használni kívánt közzétételi profil a „com.mycompany.myapp” alkalmazásazonosítót használja.</span><span class="sxs-lookup"><span data-stu-id="b15c9-124">Make sure that the publishing profile you eventually use to deploy your iOS app is using 'com.mycompany.myapp' as the App ID.</span></span> 
   
        $ cordova create azme-cordova com.mycompany.myapp
        $ cd azme-cordova
2. <span data-ttu-id="b15c9-125">Hajtsa végre az alábbiakat a projekt konfigurálásához **iOS** esetén, majd futtassa azt az iOS Simulator alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="b15c9-125">Execute the following to configure your project for **iOS** and run it in the iOS Simulator:</span></span>
   
        $ cordova platform add ios 
        $ cordova run ios
3. <span data-ttu-id="b15c9-126">Hajtsa végre az alábbiakat a projekt konfigurálásához **Android** esetén, majd futtassa azt az Android-emulátorban.</span><span class="sxs-lookup"><span data-stu-id="b15c9-126">Execute the following to configure your project for **Android** and run it in the Android emulator.</span></span> <span data-ttu-id="b15c9-127">Győződjön meg róla, hogy az Android SDK Emulator beállításainál a Target (Cél) beállítás értéke Google APIs (Google Inc.), a CPU/ABI értéke pedig a Google APIs ARM.</span><span class="sxs-lookup"><span data-stu-id="b15c9-127">Make sure that your Android SDK Emulator settings have its Target as Google APIs (Google Inc.) with the CPU / ABI as Google APIs ARM.</span></span>  
   
        $ cordova platform add android
        $ cordova run android
4. <span data-ttu-id="b15c9-128">Adja hozzá a Cordova-konzol beépülő modulját.</span><span class="sxs-lookup"><span data-stu-id="b15c9-128">Add the Cordova Console plugin.</span></span> 

    ```
    $ cordova plugin add cordova-plugin-console
    ``` 

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="b15c9-129">Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="b15c9-129">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="b15c9-130">Telepítse az Azure Mobile Engagement Cordova beépülő modulját, és közben adja meg a beépülő modul konfigurálásához szükséges változók értékeit:</span><span class="sxs-lookup"><span data-stu-id="b15c9-130">Install the Azure Mobile Engagement Cordova plugin while providing the variable values to configure the plugin:</span></span>
   
        cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
             --variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
            --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
            --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
            --variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
            --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
            --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
            --variable AZME_ENABLE_NATIVE_LOG=true|false
            --variable AZME_ENABLE_PLUGIN_LOG=true|false

<span data-ttu-id="b15c9-131">*Android Reach Icon*: az erőforrás nevének kell lennie kiterjesztés vagy a rajzolható előtag nélkül (például: mynotificationicon), és az ikonfájlt az Android-projektbe kell másolni (platforms/android/res/drawable).</span><span class="sxs-lookup"><span data-stu-id="b15c9-131">*Android Reach Icon* : must be the name of the resource without any extension, nor drawable prefix (ex: mynotificationicon), and the icon file must be copied into your android project (platforms/android/res/drawable)</span></span>

<span data-ttu-id="b15c9-132">*iOS Reach Icon*: az erőforrás nevének kell lennie a kiterjesztéssel együtt (például: mynotificationicon.png), és az ikonfájlt hozzá kell adni az iOS-projekthez az XCode segítségével (az Add Files Menu (Fájl hozzáadása menü) használatával).</span><span class="sxs-lookup"><span data-stu-id="b15c9-132">*iOS Reach Icon*  : must be the name of the resource with its extension (ex:  mynotificationicon.png), and the icon file must be added into your iOS project with XCode (using the Add Files Menu)</span></span>

## <span data-ttu-id="b15c9-133"><a id="monitor"></a>Valós idejű figyelés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b15c9-133"><a id="monitor"></a>Enabling real-time monitoring</span></span>
1. <span data-ttu-id="b15c9-134">A Cordova-projektben módosítsa a **www/js/index.js** fájlt úgy, hogy hozzáadja a Mobile Engagementre irányuló hívást egy új tevékenység deklaráláshoz a *deviceReady* esemény fogadását követően.</span><span class="sxs-lookup"><span data-stu-id="b15c9-134">In the Cordova project - edit **www/js/index.js** to add the call to Mobile Engagement to declare a new activity once the *deviceReady* event is received.</span></span>
   
         onDeviceReady: function() {
                Engagement.startActivity("myPage",{});
            }
2. <span data-ttu-id="b15c9-135">Futtassa az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="b15c9-135">Run the application:</span></span>
   
   * <span data-ttu-id="b15c9-136">**iOS esetén**</span><span class="sxs-lookup"><span data-stu-id="b15c9-136">**For iOS**</span></span>
     
       <span data-ttu-id="b15c9-137">A `Terminal` ablakban indítsa el az alkalmazást a Simulator új példányában az alábbiak végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="b15c9-137">In `Terminal` window launch your app in a new Simulator instance by executing the following:</span></span>
     
           cordova run ios
   * <span data-ttu-id="b15c9-138">**Android esetén**</span><span class="sxs-lookup"><span data-stu-id="b15c9-138">**For Android**</span></span>
     
       <span data-ttu-id="b15c9-139">A `Terminal` ablakban indítsa el az alkalmazást az emulátor új példányában az alábbiak végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="b15c9-139">In `Terminal` window launch your app in a new emulator instance by executing the following:</span></span>
     
           cordova run android
3. <span data-ttu-id="b15c9-140">A konzolnaplófájlokban az alábbiakat láthatja:</span><span class="sxs-lookup"><span data-stu-id="b15c9-140">You can see the following in the console logs:</span></span>
   
        [Engagement] Agent: Session started
        [Engagement] Agent: Activity 'myPage' started
        [Engagement] Connection: Established
        [Engagement] Connection: Sent: appInfo
        [Engagement] Connection: Sent: startSession
        [Engagement] Connection: Sent: activity name='myPage'

## <span data-ttu-id="b15c9-141"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="b15c9-141"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="b15c9-142"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b15c9-142"><a id="integrate-push"></a>Enabling Push Notifications and in-app messaging</span></span>
<span data-ttu-id="b15c9-143">A Mobile Engagement lehetővé teszi a felhasználókkal folytatott interakciót a kampányok részeként megjelenő leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel.</span><span class="sxs-lookup"><span data-stu-id="b15c9-143">Mobile Engagement allows you to interact with your users using Push Notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="b15c9-144">Ez a modul REACH (Elérés) néven érhető el a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="b15c9-144">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="b15c9-145">Az alábbi szakaszok állítják be az alkalmazást a fogadásukra.</span><span class="sxs-lookup"><span data-stu-id="b15c9-145">The following sections will setup your app to receive them.</span></span>

### <a name="configure-push-credentials-for-mobile-engagement"></a><span data-ttu-id="b15c9-146">Leküldési hitelesítő adatok konfigurálása a Mobile Engagementhez</span><span class="sxs-lookup"><span data-stu-id="b15c9-146">Configure Push credentials for Mobile Engagement</span></span>
<span data-ttu-id="b15c9-147">Annak engedélyezéséhez, hogy a Mobile Engagement leküldéses értesítéseket küldhessen a nevében, hozzáférést kell biztosítania számára az Apple iOS-tanúsítvány vagy a GCM-kiszolgáló API-kulcsához.</span><span class="sxs-lookup"><span data-stu-id="b15c9-147">To allow Mobile Engagement to send Push Notifications on your behalf, you need to grant it access to your Apple iOS certificate or GCM Server API Key.</span></span> 

1. <span data-ttu-id="b15c9-148">Nyissa meg a Mobile Engagement portált.</span><span class="sxs-lookup"><span data-stu-id="b15c9-148">Navigate to your Mobile Engagement portal.</span></span> <span data-ttu-id="b15c9-149">Nyissa meg a projekthez használt alkalmazást, majd kattintson a lap alján található **Engage** (Aktiválás) gombra:</span><span class="sxs-lookup"><span data-stu-id="b15c9-149">Ensure you're in the app we're using for this project and then click on the **Engage** button at the bottom:</span></span>
   
    ![][1]
2. <span data-ttu-id="b15c9-150">Ezzel megnyitja az Engagement portál beállításokat tartalmazó lapját.</span><span class="sxs-lookup"><span data-stu-id="b15c9-150">You will land in the settings page in your Engagement Portal.</span></span> <span data-ttu-id="b15c9-151">A lapon kattintson a **Native Push** (Natív leküldés) szakaszra:</span><span class="sxs-lookup"><span data-stu-id="b15c9-151">From there click on the **Native Push** section:</span></span>
   
    ![][2]
3. <span data-ttu-id="b15c9-152">iOS-tanúsítvány/GCM-kiszolgáló API-kulcsának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b15c9-152">Configure iOS Certificate/GCM Server API Key</span></span>
   
    <span data-ttu-id="b15c9-153">**iOS**</span><span class="sxs-lookup"><span data-stu-id="b15c9-153">**[iOS]**</span></span>
   
    <span data-ttu-id="b15c9-154">a.</span><span class="sxs-lookup"><span data-stu-id="b15c9-154">a.</span></span> <span data-ttu-id="b15c9-155">Jelölje ki .p12 tanúsítványát, töltse fel, és írja be jelszavát:</span><span class="sxs-lookup"><span data-stu-id="b15c9-155">Select your .p12, upload it and type your password:</span></span>
   
    ![][3]
   
    <span data-ttu-id="b15c9-156">**Android**</span><span class="sxs-lookup"><span data-stu-id="b15c9-156">**[Android]**</span></span>
   
    <span data-ttu-id="b15c9-157">a.</span><span class="sxs-lookup"><span data-stu-id="b15c9-157">a.</span></span> <span data-ttu-id="b15c9-158">Kattintson az **API Key** (API-kulcs) előtt található szerkesztési ikonra a GCM Settings (GCM-beállítások) szakaszban, illessze be a GCM kiszolgálói kulcsot az előugró ablakban, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b15c9-158">Click the edit icon in front of **API Key** in the GCM Settings section and in the popup which shows up, paste the GCM Server Key and click **OK**.</span></span> 
   
    ![][4]

### <a name="enable-push-notifications-in-the-cordova-app"></a><span data-ttu-id="b15c9-159">Leküldéses értesítések engedélyezése a Cordova-alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="b15c9-159">Enable push notifications in the Cordova app</span></span>
<span data-ttu-id="b15c9-160">Módosítsa a **www/js/index.js** fájlt úgy, hogy hozzáadja a Mobile Engagementre irányuló hívást leküldéses értesítések kéréséhez és egy kezelő deklarálásához:</span><span class="sxs-lookup"><span data-stu-id="b15c9-160">Edit **www/js/index.js** to add the call to Mobile Engagement to request push notifications and declare a handler:</span></span>

     onDeviceReady: function() {
           Engagement.initializeReach(  
                 // on OpenUrl  
                 function(_url) {   
                 alert(_url);   
                 });  
            Engagement.startActivity("myPage",{});  
        }

### <a name="run-the-app"></a><span data-ttu-id="b15c9-161">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="b15c9-161">Run the app</span></span>
<span data-ttu-id="b15c9-162">**iOS**</span><span class="sxs-lookup"><span data-stu-id="b15c9-162">**[iOS]**</span></span>

1. <span data-ttu-id="b15c9-163">Mivel az iOS csak tényleges eszközre teszi lehetővé leküldéses értesítések küldését, az XCode használatával fogjuk létrehozni és üzembe helyezni az alkalmazást az eszközön a leküldéses értesítések teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b15c9-163">We will use XCode to build and deploy the app on the device to test push notifications since iOS only allows push notifications to an actual device.</span></span> <span data-ttu-id="b15c9-164">Lépjen a Cordova-projekt létrehozásának helyére, majd navigáljon a **...\platforms\ios** helyre.</span><span class="sxs-lookup"><span data-stu-id="b15c9-164">Go to the location where your Cordova project is created and navigate to **...\platforms\ios** location.</span></span> <span data-ttu-id="b15c9-165">Nyissa meg a natív .xcodeproj fájlt az XCode-ban.</span><span class="sxs-lookup"><span data-stu-id="b15c9-165">Open up the native .xcodeproj file in XCode.</span></span> 
2. <span data-ttu-id="b15c9-166">Hozza létre és helyezze üzembe a Cordova-alkalmazást az iOS eszközön annak a fióknak a használatával, amely tartalmazza a Mobile Engagement portálra most feltöltött tanúsítványt és a Cordova-alkalmazás létrehozásakor megadott azonosítóval egyező alkalmazásazonosítót magában foglaló kiépítési profilt.</span><span class="sxs-lookup"><span data-stu-id="b15c9-166">Build and deploy the Cordova app to the iOS device using the account which has the provisioning profile containing the certificate you just uploaded to the Mobile Engagement portal and the App Id which matches the one you provided while creating the Cordova app.</span></span> <span data-ttu-id="b15c9-167">Az egyeztetéshez megtekintheti a *csomagazonosítót* a **Resources\*-info.plist** fájlban az XCode-ban.</span><span class="sxs-lookup"><span data-stu-id="b15c9-167">You can check out the *Bundle identifier* in your **Resources\*-info.plist** file in XCode to match it up.</span></span> 
3. <span data-ttu-id="b15c9-168">Az eszközön megjelenik a szokásos iOS előugró ablak, amely engedélyt kér az alkalmazás számára értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="b15c9-168">You will see the standard iOS popup on your device saying that the app requests permission to send notifications.</span></span> <span data-ttu-id="b15c9-169">Adja meg az engedélyt.</span><span class="sxs-lookup"><span data-stu-id="b15c9-169">Grant the permission.</span></span> 

<span data-ttu-id="b15c9-170">**Android**</span><span class="sxs-lookup"><span data-stu-id="b15c9-170">**[Android]**</span></span>

<span data-ttu-id="b15c9-171">Nyugodtan használhatja az emulátort az Android-alkalmazás futtatásához, mivel az Android-emulátor támogatja a GCM-értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="b15c9-171">You can simply use the emulator to run the Android app as GCM notifications are supported on the Android emulator.</span></span> 

    cordova run android

## <span data-ttu-id="b15c9-172"><a id="send"></a>Értesítés küldése az alkalmazásnak</span><span class="sxs-lookup"><span data-stu-id="b15c9-172"><a id="send"></a>Send a notification to your app</span></span>
<span data-ttu-id="b15c9-173">Most létre fogunk hozni egy egyszerű leküldéses értesítési kampányt, amely egy leküldéses üzenetet fog küldeni az eszközön futó alkalmazásnak:</span><span class="sxs-lookup"><span data-stu-id="b15c9-173">We will now create a simple Push Notification campaign that will send a push to your app running on the device:</span></span>

1. <span data-ttu-id="b15c9-174">Lépjen a Mobile Engagement portál **Reach** (Elérés) lapjára.</span><span class="sxs-lookup"><span data-stu-id="b15c9-174">Navigate to the **Reach** tab in your Mobile Engagement portal</span></span>
2. <span data-ttu-id="b15c9-175">Kattintson a **New Announcement** (Új közlemény) elemre a leküldéses kampány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b15c9-175">Click **New Announcement** to create your push campaign</span></span>
   
    ![][6]
3. <span data-ttu-id="b15c9-176">Adja meg a bemeneti adatokat a kampány létrehozásához **[Android]**.</span><span class="sxs-lookup"><span data-stu-id="b15c9-176">Provide inputs to create your campaign **[Android]**</span></span>
   
   * <span data-ttu-id="b15c9-177">Adja meg a kampány nevét a **Name** mezőben.</span><span class="sxs-lookup"><span data-stu-id="b15c9-177">Provide a **Name** for your campaign.</span></span> 
   * <span data-ttu-id="b15c9-178">A **Delivery Type** (Kézbesítési típus) értékeként jelölje be a *System notification* és *Simple* (Rendszerértesítés, Egyszerű) választógombot.</span><span class="sxs-lookup"><span data-stu-id="b15c9-178">Select the **Delivery Type** as *System notification* *Simple*</span></span>
   * <span data-ttu-id="b15c9-179">A **Delivery time** (Kézbesítési idő) értékeként jelölje be az *Any time* (Bármikor) választógombot.</span><span class="sxs-lookup"><span data-stu-id="b15c9-179">Select the **Delivery time** as *"Any Time"*</span></span>
   * <span data-ttu-id="b15c9-180">Írja be az értesítés címét a **Title** (Cím) mezőbe. A cím a leküldéses értesítés első sora lesz.</span><span class="sxs-lookup"><span data-stu-id="b15c9-180">Provide a **Title** for your notification which will be the first line in the push.</span></span>
   * <span data-ttu-id="b15c9-181">Írja be az értesítés szövegét a **Message** (Üzenet) mezőbe. Ez a szöveg lesz az üzenet törzse.</span><span class="sxs-lookup"><span data-stu-id="b15c9-181">Provide a **Message** for your notification which will serve as the message body.</span></span> 
     
     ![][11]
4. <span data-ttu-id="b15c9-182">Adja meg a bemeneti adatokat a kampány létrehozásához **[iOS]**.</span><span class="sxs-lookup"><span data-stu-id="b15c9-182">Provide inputs to create your campaign **[iOS]**</span></span>
   
   * <span data-ttu-id="b15c9-183">Írja be a kampány nevét a **Name** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="b15c9-183">Provide a **Name** for your campaign.</span></span> 
   * <span data-ttu-id="b15c9-184">A **Delivery time** (Kézbesítési idő) értékeként jelölje be az *Out of app only* (Csak alkalmazáson kívül) választógombot.</span><span class="sxs-lookup"><span data-stu-id="b15c9-184">Select the **Delivery time** as *"Out of app only"*</span></span>
   * <span data-ttu-id="b15c9-185">Írja be az értesítés címét a **Title** (Cím) mezőbe. A cím a leküldéses értesítés első sora lesz.</span><span class="sxs-lookup"><span data-stu-id="b15c9-185">Provide a **Title** for your notification which will be the first line in the push.</span></span>
   * <span data-ttu-id="b15c9-186">Írja be az értesítés szövegét a **Message** (Üzenet) mezőbe. Ez a szöveg lesz az üzenet törzse.</span><span class="sxs-lookup"><span data-stu-id="b15c9-186">Provide a **Message** for your notification which will serve as the message body.</span></span> 
     
     ![][12]
5. <span data-ttu-id="b15c9-187">Görgessen le, és a Content (Tartalom) részen jelölje be a **Notification only** (Csak értesítés) választógombot.</span><span class="sxs-lookup"><span data-stu-id="b15c9-187">Scroll down, and in the content section select **Notification only**</span></span>
   
    ![][8]
6. <span data-ttu-id="b15c9-188">(Választható) Megadhatja egy művelet URL-címét is.</span><span class="sxs-lookup"><span data-stu-id="b15c9-188">[Optional] You can also provide an Action URL.</span></span> <span data-ttu-id="b15c9-189">Győződjön meg róla, hogy a cím a beépülő modul **AZME\_REDIRECT\_URL** változójának konfigurálásánál megadott URL-sémát használja, például: *myapp://test*.</span><span class="sxs-lookup"><span data-stu-id="b15c9-189">Make sure that it uses a URL scheme provided while configuring the plugin's **AZME\_REDIRECT\_URL** variable e.g. *myapp://test*.</span></span>  
7. <span data-ttu-id="b15c9-190">Ezzel befejezte a lehető legalapvetőbb kampány beállítását.</span><span class="sxs-lookup"><span data-stu-id="b15c9-190">You're done setting the most basic campaign possible.</span></span> <span data-ttu-id="b15c9-191">Görgessen újra le, és kattintson a **Create** (Létrehozás) gombra a kampány mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b15c9-191">Now scroll down again and click the **Create** button to save your campaign.</span></span>
8. <span data-ttu-id="b15c9-192">Végül aktiválja a kampányt az **Activate** (Aktiválás) gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="b15c9-192">Finally **Activate** your campaign</span></span>
   
    ![][10]
9. <span data-ttu-id="b15c9-193">Ekkor meg kellene jelennie egy leküldéses értesítésnek az eszközön vagy az emulátorban a jelen kampány részeként.</span><span class="sxs-lookup"><span data-stu-id="b15c9-193">You should now see a push notification on your device or emulator as part of this campaign.</span></span> 

## <span data-ttu-id="b15c9-194"><a id="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b15c9-194"><a id="next-steps"></a>Next Steps</span></span>
[<span data-ttu-id="b15c9-195">A Cordova Mobile Engagement SDK-val elérhető összes módszer áttekintése</span><span class="sxs-lookup"><span data-stu-id="b15c9-195">Overview of all methods available with Cordova Mobile Engagement SDK</span></span>](https://github.com/Azure/azure-mobile-engagement-cordova)

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

