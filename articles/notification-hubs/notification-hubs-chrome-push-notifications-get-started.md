---
title: "Leküldéses értesítések küldése Chrome-alkalmazásokba az Azure Notification Hubs használatával | Microsoft Docs"
description: "Ebből az anyagból megtudhatja, hogyan küldhet leküldéses értesítéseket Chrome-alkalmazásokba az Azure Notification Hubs használatával."
services: notification-hubs
keywords: "mobil leküldéses értesítések,leküldéses értesítések,leküldéses értesítés,chrome leküldéses értesítések"
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 600b1b7e5f3987c9a0acc33b7049f7118442b931
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a><span data-ttu-id="02fe8-104">Leküldéses értesítések küldése Chrome-alkalmazásokba az Azure Notification Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="02fe8-104">Send push notifications to Chrome apps with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

<span data-ttu-id="02fe8-105">Ebből a témakörből megtudhatja, hogyan küldhet leküldéses értesítéseket Chrome-alkalmazásokba az Azure Notification Hubs használatával. Az alkalmazás a Google Chrome böngészőben jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="02fe8-105">This topic shows you how to use Azure Notification Hubs to send push notifications to a Chrome App, which will be displayed within the context of the Google Chrome browser.</span></span> <span data-ttu-id="02fe8-106">Ebben az oktatóanyagban létrehozunk egy olyan Chrome-alkalmazást, amely leküldéses értesítéseket fogad a [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/) használatával.</span><span class="sxs-lookup"><span data-stu-id="02fe8-106">In this tutorial, we will create a Chrome app that receives push notifications by using [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/).</span></span> 

> [!NOTE]
> <span data-ttu-id="02fe8-107">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="02fe8-107">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="02fe8-108">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="02fe8-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="02fe8-109">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="02fe8-109">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="02fe8-110">Az oktatóanyag bemutatja a leküldéses értesítések engedélyezéséhez szükséges, alapvető lépéseket:</span><span class="sxs-lookup"><span data-stu-id="02fe8-110">The tutorial walks you through these basic steps to enable push notifications:</span></span>

* [<span data-ttu-id="02fe8-111">A Google Cloud Messaging engedélyezése</span><span class="sxs-lookup"><span data-stu-id="02fe8-111">Enable Google Cloud Messaging</span></span>](#register)
* [<span data-ttu-id="02fe8-112">Az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="02fe8-112">Configure your notification hub</span></span>](#configure-hub)
* [<span data-ttu-id="02fe8-113">A Chrome-alkalmazás csatlakoztatása az értesítési központhoz</span><span class="sxs-lookup"><span data-stu-id="02fe8-113">Connect your Chrome App to the notification hub</span></span>](#connect-app)
* [<span data-ttu-id="02fe8-114">Leküldéses értesítés küldése a Chrome-alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="02fe8-114">Send a push notification to your Chrome App</span></span>](#send)
* [<span data-ttu-id="02fe8-115">További funkciók és képességek</span><span class="sxs-lookup"><span data-stu-id="02fe8-115">Additional functionality & capabilities</span></span>](#next-steps)

> [!NOTE]
> <span data-ttu-id="02fe8-116">A Chrome-alkalmazások leküldéses értesítései nem általános böngészőn belüli értesítések, hanem a böngésző bővíthetőségi modelljére jellemzőek (a részletes információkat lásd: [Chrome-alkalmazások – áttekintés]).</span><span class="sxs-lookup"><span data-stu-id="02fe8-116">Chrome app push notifications are not generic in-browser notifications - they are specific to the browser extensibility model (see [Chrome Apps Overview] for details).</span></span> <span data-ttu-id="02fe8-117">Az asztali böngészőn kívül a Chrome-alkalmazások (Android és iOS rendszerű) mobileszközökön is futnak az Apache Cordova segítségével.</span><span class="sxs-lookup"><span data-stu-id="02fe8-117">In addition to the desktop browser, Chrome apps run on mobile (Android and iOS) through Apache Cordova.</span></span> <span data-ttu-id="02fe8-118">További információk: [Chrome-alkalmazások mobileszközökön].</span><span class="sxs-lookup"><span data-stu-id="02fe8-118">See [Chrome Apps on Mobile] to learn more.</span></span>
> 
> 

<span data-ttu-id="02fe8-119">A GCM és az Azure Notification Hubs konfigurálása ugyanúgy történik, mint Androidon, mivel a [Google Cloud Messaging for Chrome] elavult, így az Android-eszközöket és a Chrome-példányokat már ugyanaz a GCM támogatja.</span><span class="sxs-lookup"><span data-stu-id="02fe8-119">Configuring GCM and Azure Notification Hubs is identical to configuring for Android, since [Google Cloud Messaging for Chrome] has been deprecated and the same GCM now supports both Android devices and Chrome instances.</span></span>

## <span data-ttu-id="02fe8-120"><a id="register"></a>A Google Cloud Messaging engedélyezése</span><span class="sxs-lookup"><span data-stu-id="02fe8-120"><a id="register"></a>Enable Google Cloud Messaging</span></span>
1. <span data-ttu-id="02fe8-121">Nyissa meg a [Google Cloud Console] webhelyét, jelentkezzen be Google-fiókja hitelesítő adataival, majd kattintson a **Create Project** (Projekt létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="02fe8-121">Navigate to the [Google Cloud Console] website, sign in with your Google account credentials, and then click the **Create Project** button.</span></span> <span data-ttu-id="02fe8-122">Adjon meg egy megfelelő **projektnevet**, majd kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="02fe8-122">Provide an appropriate **Project Name**, and then click the **Create** button.</span></span>
   
       ![Google Cloud Console - Create Project][1]
2. <span data-ttu-id="02fe8-123">Jegyezze fel a most létrehozott projekt **Projects** (Projektek) oldalán szereplő **Project Number** (Projektszám) értékét.</span><span class="sxs-lookup"><span data-stu-id="02fe8-123">Make a note of the **Project Number** on the **Projects** page for the project that you just created.</span></span> <span data-ttu-id="02fe8-124">Ezt a Chrome-alkalmazás GCM-regisztrációja során fogja használni a **GCM Sender ID** (GCM-küldőazonosító) értékeként.</span><span class="sxs-lookup"><span data-stu-id="02fe8-124">You will use this as the **GCM Sender ID** in the Chrome App to register with GCM.</span></span>
   
       ![Google Cloud Console - Project Number][2]
3. <span data-ttu-id="02fe8-125">A bal oldali ablaktáblán kattintson az **APIs & auth** (API-k és hitelesítés) elemre, majd görgessen lefelé, és kattintással engedélyezze a **Google Cloud Messaging for Android** beállítást.</span><span class="sxs-lookup"><span data-stu-id="02fe8-125">In the left pane, click **APIs & auth**, and then scroll down and click the toggle to enable **Google Cloud Messaging for Android**.</span></span> <span data-ttu-id="02fe8-126">A **Google Cloud Messaging for Chrome** használatát nem kell külön engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="02fe8-126">You don't have to enable **Google Cloud Messaging for Chrome**.</span></span>
   
       ![Google Cloud Console - Server Key][3]
4. <span data-ttu-id="02fe8-127">A bal oldali ablaktáblán kattintson a **Credentials** (Hitelesítő adatok)  > **Create New Key** (Új kulcs létrehozása)  > **Server Key** (Kiszogálókulcs)  > **Create** (Létrehozás) elemre.</span><span class="sxs-lookup"><span data-stu-id="02fe8-127">In the left pane, click **Credentials** > **Create New Key** > **Server Key** > **Create**.</span></span>
   
       ![Google Cloud Console - Credentials][4]
5. <span data-ttu-id="02fe8-128">Jegyezze fel a kiszolgálói **API KEY** (API-kulcs) értékét.</span><span class="sxs-lookup"><span data-stu-id="02fe8-128">Make a note of the server **API Key**.</span></span> <span data-ttu-id="02fe8-129">Ezt az értesítési központban fogja konfigurálni, így engedélyezheti a leküldéses értesítések küldését a GCM-be.</span><span class="sxs-lookup"><span data-stu-id="02fe8-129">You will configure this in your notification hub next, to enable it to send push notifications to GCM.</span></span>
   
       ![Google Cloud Console - API Key][5]

## <span data-ttu-id="02fe8-130"><a id="configure-hub"></a>A saját értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="02fe8-130"><a id="configure-hub"></a>Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="02fe8-131">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="02fe8-131">&emsp;&emsp;6.</span></span>   <span data-ttu-id="02fe8-132">A **Beállítások** panelen válassza az **Értesítési szolgáltatások**, majd a **Google (GCM)** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="02fe8-132">In the **Settings** blade, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="02fe8-133">Adja meg az API-kulcsot, majd mentsen.</span><span class="sxs-lookup"><span data-stu-id="02fe8-133">Enter the API key and save.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <span data-ttu-id="02fe8-135"><a id="connect-app"></a>A Chrome-alkalmazás csatlakoztatása az értesítési központhoz</span><span class="sxs-lookup"><span data-stu-id="02fe8-135"><a id="connect-app"></a>Connect your Chrome App to the notification hub</span></span>
<span data-ttu-id="02fe8-136">Az értesítési központ konfigurálva lett a GCM-mel való együttműködésre, és rendelkezik a kapcsolati karakterláncokkal az alkalmazás regisztrálására értesítések fogadásához és leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="02fe8-136">Your notification hub is now configured to work with GCM, and you have the connection strings to register your app to both receive and send push notifications.</span></span> <span data-ttu-id="02fe8-137">LK</span><span class="sxs-lookup"><span data-stu-id="02fe8-137">LK</span></span>

### <a name="create-a-new-chrome-app"></a><span data-ttu-id="02fe8-138">Új Chrome-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="02fe8-138">Create a new Chrome App</span></span>
<span data-ttu-id="02fe8-139">Az alábbi minta alapjául a [Chrome-alkalmazások GCM mintája] szolgál, és a Chrome-alkalmazás javasolt létrehozási módját alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="02fe8-139">The sample below is based on the [Chrome App GCM Sample] and uses the recommended way to create a Chrome App.</span></span> <span data-ttu-id="02fe8-140">A kifejezetten az Azure Notification Hubsra vonatkozó lépések ki vannak emelve.</span><span class="sxs-lookup"><span data-stu-id="02fe8-140">We will highlight the steps specifically related to Azure Notification Hubs.</span></span> 

> [!NOTE]
> <span data-ttu-id="02fe8-141">Javasoljuk, hogy töltse le a jelen Chrome-alkalmazás forrását a következő helyről: [Chrome-alkalmazás – Notification Hub-minta].</span><span class="sxs-lookup"><span data-stu-id="02fe8-141">We recommend that you download the source for this Chrome App from [Chrome App Notification Hub Sample].</span></span>
> 
> 

<span data-ttu-id="02fe8-142">A Chrome-alkalmazás JavaScripttel készült, és bármilyen szövegszerkesztővel létrehozható.</span><span class="sxs-lookup"><span data-stu-id="02fe8-142">The Chrome App is created via JavaScript, and you can use any of your preferred word editors for creating it.</span></span> <span data-ttu-id="02fe8-143">A Chrome-alkalmazás a következőképpen fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="02fe8-143">Below is what this Chrome App will look like.</span></span>

![Google Chrome-alkalmazás][15]

1. <span data-ttu-id="02fe8-145">Hozzon létre egy `ChromePushApp` nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="02fe8-145">Create a folder and name it `ChromePushApp`.</span></span> <span data-ttu-id="02fe8-146">A név természetesen szabadon választható – ha más nevet ad neki, akkor azt használja szükséges kódszakaszok elérési útjában is.</span><span class="sxs-lookup"><span data-stu-id="02fe8-146">Of course, the name is arbitrary - if you name it something different, make sure you substitute the path in the required code segments.</span></span>
2. <span data-ttu-id="02fe8-147">A második lépésben létrehozott mappába töltse le a [crypto-js kódtárat].</span><span class="sxs-lookup"><span data-stu-id="02fe8-147">Download the [crypto-js library] in the folder you created in the second step.</span></span> <span data-ttu-id="02fe8-148">Ez a kódtármappa a következő két almappát tartalmazza: `components` és `rollups`.</span><span class="sxs-lookup"><span data-stu-id="02fe8-148">This library folder will contain two subfolders: `components` and `rollups`.</span></span>
3. <span data-ttu-id="02fe8-149">Hozzon létre egy `manifest.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="02fe8-149">Create a `manifest.json` file.</span></span> <span data-ttu-id="02fe8-150">Az összes Chrome-alkalmazás egy olyan jegyzékfájlt használ, amely tartalmazza az alkalmazás metaadatait, és –ami még fontosabb – a felhasználói telepítés során az alkalmazásnak megadott összes engedélyt.</span><span class="sxs-lookup"><span data-stu-id="02fe8-150">All Chrome Apps are backed by a manifest file that contains the app metadata and, most importantly, all permissions that are granted to the app when the user installs it.</span></span>
   
        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }
   
    <span data-ttu-id="02fe8-151">Figyelje meg a `permissions` elemet, amely megadja, hogy a Chrome-alkalmazás fogadhat-e leküldéses értesítéseket a GCM-től.</span><span class="sxs-lookup"><span data-stu-id="02fe8-151">Notice the `permissions` element, which specifies that this Chrome App will be able to receive push notifications from GCM.</span></span> <span data-ttu-id="02fe8-152">Meg kell adnia az Azure Notification Hubs URI-ját is, ahol a Chrome-alkalmazás REST-hívást hajt végre a regisztrációhoz.</span><span class="sxs-lookup"><span data-stu-id="02fe8-152">It must also specify the Azure Notification Hubs URI where the Chrome App will make a REST call to register.</span></span>
    <span data-ttu-id="02fe8-153">Mintaalkalmazásunk a `gcm_128.png` ikonfájlt is használja, amelyet az eredeti GCM-mintából újrahasznosított forrásban talál.</span><span class="sxs-lookup"><span data-stu-id="02fe8-153">Our sample app also uses an icon file, `gcm_128.png`, that you will find at the source that's reused from the original GCM sample.</span></span> <span data-ttu-id="02fe8-154">Ezt bármilyen képpel helyettesítheti, amely megfelel az [ikonokra vonatkozó követelményeknek](https://developer.chrome.com/apps/manifest/icons).</span><span class="sxs-lookup"><span data-stu-id="02fe8-154">You can substitute it for any image that fits the [icon criteria](https://developer.chrome.com/apps/manifest/icons).</span></span>
4. <span data-ttu-id="02fe8-155">Hozzon létre egy `background.js` nevű fájlt a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="02fe8-155">Create a file called `background.js` with the following code:</span></span>
   
        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }
   
        var registerWindowCreated = false;
   
        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {
   
            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }
   
        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);
   
        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    <span data-ttu-id="02fe8-156">Ez a fájl ugrik fel a Chrome-alkalmazás HTML-ablakában (**register.html**), valamint meghatározza a bejövő leküldéses értesítések kezelésére szolgáló **messageReceived** kezelőt is.</span><span class="sxs-lookup"><span data-stu-id="02fe8-156">This is the file that pops up the Chrome App window HTML (**register.html**) and also defines the handler **messageReceived** to handle the incoming push notification.</span></span>
5. <span data-ttu-id="02fe8-157">Hozzon létre egy `register.html` nevű fájlt – ez határozza meg a Chrome-alkalmazás felhasználói felületét.</span><span class="sxs-lookup"><span data-stu-id="02fe8-157">Create a file called `register.html` - this defines the UI of the Chrome App.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="02fe8-158">Ebben a példában a következőt használjuk: **CryptoJS v3.1.2**.</span><span class="sxs-lookup"><span data-stu-id="02fe8-158">This sample uses **CryptoJS v3.1.2**.</span></span> <span data-ttu-id="02fe8-159">Ha a kódtár másik verzióját töltötte le, helyettesítse be a megfelelő verziószámot az `src` elérési útjában.</span><span class="sxs-lookup"><span data-stu-id="02fe8-159">If you downloaded another version of the library, make sure you properly substitute the version in the `src` path.</span></span>
   > 
   > 
   
        <html>
   
        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
   
        <body>
   
        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>
   
        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>
   
        <br/>
   
        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
   
        <br/>
        <br/>
   
        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
   
        </body>
   
        </html>
6. <span data-ttu-id="02fe8-160">Hozzon létre egy `register.js` nevű fájlt az alábbi kóddal.</span><span class="sxs-lookup"><span data-stu-id="02fe8-160">Create a file called `register.js` with the code below.</span></span> <span data-ttu-id="02fe8-161">Ez a fájl a `register.html` szkriptjét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="02fe8-161">This file specifies the script behind `register.html`.</span></span> <span data-ttu-id="02fe8-162">A Chrome-alkalmazások nem engedélyezik a beágyazott futtatás használatát, így a felhasználói felülethez külön háttérszkriptet kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="02fe8-162">Chrome Apps do not allow inline execution, so you have to create a separate backing script for your UI.</span></span>
   
        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";
   
        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }
   
        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }
   
          document.getElementById("console").innerHTML = currentStatus  + status;
        }
   
        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);
   
          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }
   
        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }
   
        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";
   
          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });
   
          originalUri = endpoint + hubName;
        }
   
        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration
   
          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;
   
          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);
   
          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }
   
        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";
   
          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);
   
          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();
   
          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };
   
          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }
   
          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");
   
          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }
   
    <span data-ttu-id="02fe8-163">A fenti szkript a következő fő paraméterekkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="02fe8-163">The above script has the following key parameters:</span></span>
   
   * <span data-ttu-id="02fe8-164">A **window.onload** határozza meg két gomb kattintási eseményeit a felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="02fe8-164">**window.onload** defines the button-click events of the two buttons on the UI.</span></span> <span data-ttu-id="02fe8-165">Az egyik a GCM-ben regisztrál, a másik pedig a másik a GCM-regisztráció után visszaadott regisztrációs azonosítót az Azure Notification Hubs-regisztrációra használja.</span><span class="sxs-lookup"><span data-stu-id="02fe8-165">One registers with GCM, and the other uses the registration ID that's returned after registration with GCM to register with Azure Notification Hubs.</span></span>
   * <span data-ttu-id="02fe8-166">Az **updateLog** egyszerű naplózási képességeket biztosító függvény.</span><span class="sxs-lookup"><span data-stu-id="02fe8-166">**updateLog** is the function that allows us to handle simple logging capabilities.</span></span>
   * <span data-ttu-id="02fe8-167">A **registerWithGCM** az első kattintáskezelő, amely végrehajtja a `chrome.gcm.register` hívást a GCM felé az aktuális Chrome App-példány regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="02fe8-167">**registerWithGCM** is the first button-click handler, which makes the `chrome.gcm.register` call to GCM to register the current Chrome App instance.</span></span>
   * <span data-ttu-id="02fe8-168">A **registerCallback** visszahívási függvényt a rendszer a GCM-regisztrációs hívás visszatérésekor hívja meg.</span><span class="sxs-lookup"><span data-stu-id="02fe8-168">**registerCallback** is the callback function that gets called when the GCM registration call returns.</span></span>
   * <span data-ttu-id="02fe8-169">A **registerWithNH** a második kattintáskezelő, amely a Notification Hubs-regisztrációt hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="02fe8-169">**registerWithNH** is the second button-click handler, which registers with Notification Hubs.</span></span> <span data-ttu-id="02fe8-170">Lekéri a `hubName` és (a felhasználó által megadott ) `connectionString` paramétereket, valamint létrehozza a Notification Hubs-regisztrációs REST API-hívást.</span><span class="sxs-lookup"><span data-stu-id="02fe8-170">It gets `hubName` and `connectionString` (which the user has specified) and crafts the Notification Hubs Registration REST API call.</span></span>
   * <span data-ttu-id="02fe8-171">A **splitConnectionString** és a **generateSaSToken** olyan segítők, amelyek az SaS-jogkivonatok létrehozásának JavaScript-alapú megvalósítását képviselik. Ezeket az összes REST API-hívásban használni kell.</span><span class="sxs-lookup"><span data-stu-id="02fe8-171">**splitConnectionString** and **generateSaSToken** are helpers that represent the JavaScript implementation of a SaS token creation process, that must be used in all REST API calls.</span></span> <span data-ttu-id="02fe8-172">További információk: [Általánosan használt fogalmak](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="02fe8-172">For more information, see [Common Concepts](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
   * <span data-ttu-id="02fe8-173">A **sendNHRegistrationRequest** függvény HTTP REST-hívást haj végre az Azure Notification Hubs felé.</span><span class="sxs-lookup"><span data-stu-id="02fe8-173">**sendNHRegistrationRequest** is the function that makes a HTTP REST call to Azure Notification Hubs.</span></span>
   * <span data-ttu-id="02fe8-174">A **registrationPayload** határozza meg a regisztrációs XML hasznos adatait.</span><span class="sxs-lookup"><span data-stu-id="02fe8-174">**registrationPayload** defines the registration XML payload.</span></span> <span data-ttu-id="02fe8-175">További információk: [Regisztrációs NH REST API létrehozása].</span><span class="sxs-lookup"><span data-stu-id="02fe8-175">For more information, see [Create Registration NH REST API].</span></span> <span data-ttu-id="02fe8-176">A benne található regisztrációs azonosítót a GCM-től kapott adatokkal frissítjük.</span><span class="sxs-lookup"><span data-stu-id="02fe8-176">We update the registration ID in it with what we received from GCM.</span></span>
   * <span data-ttu-id="02fe8-177">A **client** az **XMLHttpRequest** azon példánya, amelyet a HTTP POST-kérés végrehajtására használunk.</span><span class="sxs-lookup"><span data-stu-id="02fe8-177">**client** is an instance of **XMLHttpRequest** that we use to make the HTTP POST request.</span></span> <span data-ttu-id="02fe8-178">Figyelje meg, hogy az `Authorization` fejlécet a következővel frissítjük: `sasToken`.</span><span class="sxs-lookup"><span data-stu-id="02fe8-178">Note that we update the `Authorization` header with `sasToken`.</span></span> <span data-ttu-id="02fe8-179">A hívás sikeres végrehajtása regisztrálja a Chrome-alkalmazás ezen példányát az Azure Notification Hubsban.</span><span class="sxs-lookup"><span data-stu-id="02fe8-179">Successful completion of this call will register this Chrome App instance with Azure Notification Hubs.</span></span>

<span data-ttu-id="02fe8-180">A projekt teljes mappastruktúrája a következőhöz hasonlít: ![Google Chrome-alkalmazás – Mappastruktúra][21]</span><span class="sxs-lookup"><span data-stu-id="02fe8-180">The overall folder structure for this project should resemble this: ![Google Chrome App - Folder Structure][21]</span></span>

### <a name="set-up-and-test-your-chrome-app"></a><span data-ttu-id="02fe8-181">A Chrome-alkalmazás beállítása és tesztelése</span><span class="sxs-lookup"><span data-stu-id="02fe8-181">Set up and test your Chrome App</span></span>
1. <span data-ttu-id="02fe8-182">Nyissa meg a Chrome böngészőt.</span><span class="sxs-lookup"><span data-stu-id="02fe8-182">Open your Chrome browser.</span></span> <span data-ttu-id="02fe8-183">Nyissa meg a **Chrome-bővítményeket**, majd engedélyezze a **Fejlesztői módot**.</span><span class="sxs-lookup"><span data-stu-id="02fe8-183">Open **Chrome extensions** and enable **Developer mode**.</span></span>
   
       ![Google Chrome - Enable Developer Mode][16]
2. <span data-ttu-id="02fe8-184">Kattintson a **Kicsomagolt bővítmények betöltése** elemre, és keresse meg azt a mappát, amelyben a fájlokat létrehozta.</span><span class="sxs-lookup"><span data-stu-id="02fe8-184">Click **Load unpacked extension** and navigate to the folder where you created the files.</span></span> <span data-ttu-id="02fe8-185">Igény szerint a **Chrome-alkalmazások és bővítmények fejlesztői eszközét** is használhatja.</span><span class="sxs-lookup"><span data-stu-id="02fe8-185">You can also optionally use the **Chrome Apps & Extensions Developer Tool**.</span></span> <span data-ttu-id="02fe8-186">Ez az eszköz maga is egy Chrome-alkalmazás, amely a Chrome webáruházból telepíthető, és speciális hibakeresési képességeket biztosít a Chrome-alkalmazások fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="02fe8-186">This tool is a Chrome App in itself (installed from the Chrome Web Store) and provides advanced debugging capabilities for your Chrome App development.</span></span>
   
       ![Google Chrome - Load Unpacked Extension][17]
3. <span data-ttu-id="02fe8-187">Ha a Chrome-alkalmazás létrehozása hibaüzenet nélkül megtörtént, akkor megjelenik az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="02fe8-187">If the Chrome App is created without any errors, then you will see your Chrome App show up.</span></span>
   
       ![Google Chrome - Chrome App Display][18]
4. <span data-ttu-id="02fe8-188">Adja meg a **Google Cloud Console**-ból korábban beszerzett **Projektszámot** küldőazonosítóként, és kattintson a **Register with GCM** (Regisztráció a GCM-ben) elemre.</span><span class="sxs-lookup"><span data-stu-id="02fe8-188">Enter the **Project Number** that you got earlier from the **Google Cloud Console** as the sender ID, and click **Register with GCM**.</span></span> <span data-ttu-id="02fe8-189">A következő üzenetet kell látnia: **Registration with GCM succeeded** (Sikeres GCM-regisztráció).</span><span class="sxs-lookup"><span data-stu-id="02fe8-189">You must see the message **Registration with GCM succeeded.**</span></span>
   
       ![Google Chrome - Chrome App Customization][19]
5. <span data-ttu-id="02fe8-190">Adja meg a **Notification Hub Name** (Notification Hub neve) és a **DefaultListenSharedAccessSignature** értéket, amelyeket korábban szerzett be a portálról, majd kattintson a **Register with Azure Notification Hub** (Regisztráció az Azure Notification Hub használatával) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="02fe8-190">Enter your **Notification Hub Name** and the **DefaultListenSharedAccessSignature** that you obtained from the portal earlier, and click **Register with Azure Notification Hub**.</span></span> <span data-ttu-id="02fe8-191">A következő üzenetet kell látnia: **Notification Hub Registration successful!** (Sikeres Notification Hub-regisztráció), valamint</span><span class="sxs-lookup"><span data-stu-id="02fe8-191">You must see the message **Notification Hub Registration successful!**</span></span> <span data-ttu-id="02fe8-192">a regisztrációs válasz részletes adatait, amely tartalmazza az Azure Notification Hubs-regisztráció azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="02fe8-192">and the details of the registration response, which contains the Azure Notification Hubs registration ID.</span></span>
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <span data-ttu-id="02fe8-193"><a name="send"></a>Értesítés küldése a Chrome-alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="02fe8-193"><a name="send"></a>Send a notification to your Chrome App</span></span>
<span data-ttu-id="02fe8-194">Tesztelési célra a Chrome leküldéses értesítéseit egy .NET-konzolalkalmazás használatával küldjük el.</span><span class="sxs-lookup"><span data-stu-id="02fe8-194">For testing purposes, we will send Chrome push notifications by using a .NET console application.</span></span> 

> [!NOTE]
> <span data-ttu-id="02fe8-195">A Notification Hubs használatával bármilyen háttérrendszerből küldhet leküldéses értesítést a nyilvános <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-felületen</a> keresztül.</span><span class="sxs-lookup"><span data-stu-id="02fe8-195">You can send push notifications with Notification Hubs from any backend via our public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="02fe8-196">A platformok közötti átjárhatóságról további példákat a [dokumentációs portálon](https://azure.microsoft.com/documentation/services/notification-hubs/) talál.</span><span class="sxs-lookup"><span data-stu-id="02fe8-196">Check out our [documentation portal](https://azure.microsoft.com/documentation/services/notification-hubs/) for more cross-platform examples.</span></span>
> 
> 

1. <span data-ttu-id="02fe8-197">A Visual Studio **Fájl** menüjében válassza az **Új**, majd a **Projekt** elemet.</span><span class="sxs-lookup"><span data-stu-id="02fe8-197">In Visual Studio, from the **File** menu, select **New** and then **Project**.</span></span> <span data-ttu-id="02fe8-198">A **Visual C#** területen kattintson a **Windows** és a **Konzolalkalmazás**, majd pedig az **OK** elemre.</span><span class="sxs-lookup"><span data-stu-id="02fe8-198">Under **Visual C#**, click **Windows** and **Console Application**, and then click **OK**.</span></span>  <span data-ttu-id="02fe8-199">Ekkor létrejön egy új konzolalkalmazás-projekt.</span><span class="sxs-lookup"><span data-stu-id="02fe8-199">This creates a new console application project.</span></span>
2. <span data-ttu-id="02fe8-200">Az **Eszközök** menüben kattintson a **Kódtárcsomag-kezelő** majd a **Csomagkezelő konzol** elemre.</span><span class="sxs-lookup"><span data-stu-id="02fe8-200">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span> <span data-ttu-id="02fe8-201">Megjelenik a Package Manager Console (Csomagkezelő konzol) ablak.</span><span class="sxs-lookup"><span data-stu-id="02fe8-201">This displays the Package Manager Console.</span></span>
3. <span data-ttu-id="02fe8-202">A konzolablakban hajtsa végre a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="02fe8-202">In the console window, execute the following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference to the Azure Service Bus SDK with the <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. <span data-ttu-id="02fe8-203">Nyissa meg a `Program.cs` elemet, majd adja hozzá a következő `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="02fe8-203">Open `Program.cs` and add the following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="02fe8-204">A `Program` osztályban adja hozzá a következő metódust:</span><span class="sxs-lookup"><span data-stu-id="02fe8-204">In the `Program` class, add the following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure to replace the `<hub name>` placeholder with the name of the notification hub that appears in the [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace the connection string placeholder with the connection string called `DefaultFullSharedAccessSignature` that you obtained in the notification hub configuration section.
   
   > [!NOTE]
   > <span data-ttu-id="02fe8-205">A kapcsolati karakterláncot **Teljes**, és ne **Figyelési** hozzáféréssel használja.</span><span class="sxs-lookup"><span data-stu-id="02fe8-205">Make sure that you use the connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="02fe8-206">A **Figyelési** hozzáféréssel rendelkező kapcsolati karakterláncok nem biztosítanak engedélyeket a leküldéses értesítések küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="02fe8-206">The **Listen** access connection string does not grant permissions to send push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="02fe8-207">Adja hozzá a következő hívásokat a `Main` metódushoz:</span><span class="sxs-lookup"><span data-stu-id="02fe8-207">Add the following calls in the `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="02fe8-208">Győződjön meg arról, hogy a Chrome böngésző fut, majd futtassa a konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="02fe8-208">Make sure that Chrome is running, and run the console application.</span></span>
8. <span data-ttu-id="02fe8-209">Az alábbi felugró értesítésnek kell megjelennie az asztalon.</span><span class="sxs-lookup"><span data-stu-id="02fe8-209">You should see the following notification pop up on your desktop.</span></span>
   
       ![Google Chrome - Notification][13]
9. <span data-ttu-id="02fe8-210">Ha a Chrome fut, a tálca Chrome-értesítések ablakában az összes értesítést láthatja (Windowson).</span><span class="sxs-lookup"><span data-stu-id="02fe8-210">You can also see all your notifications by using the Chrome Notifications window in the taskbar (in Windows) when Chrome is running.</span></span>
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> <span data-ttu-id="02fe8-211">A Chrome-alkalmazást nem kell futtatnia vagy megnyitnia a böngészőben (de a Chrome böngészőnek futnia kell).</span><span class="sxs-lookup"><span data-stu-id="02fe8-211">You don't need to have the Chrome App running or open in the browser (though the Chrome browser itself must be running).</span></span> <span data-ttu-id="02fe8-212">A Chrome-értesítések ablakban az összes értesítés egyesített nézetét is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="02fe8-212">You also get a consolidated view of all your notifications in the Chrome Notifications window.</span></span>
> 
> 

## <span data-ttu-id="02fe8-213"><a name="next-steps"> </a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="02fe8-213"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="02fe8-214">További tudnivalók a Notification Hubsról: [Notification Hubs – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="02fe8-214">Learn more about Notification Hubs in [Notification Hubs Overview].</span></span>

<span data-ttu-id="02fe8-215">Ha adott felhasználóknak szeretne értesítést küldeni, tekintse meg az [Azure Notification Hubs – Felhasználók értesítése] oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="02fe8-215">To target specific users, refer to the [Azure Notification Hubs Notify Users] tutorial.</span></span> 

<span data-ttu-id="02fe8-216">Ha a felhasználókat érdeklődési körök alapján szeretné szegmentálni, olvassa el az [Azure Notification Hubs – Legfrissebb hírek] témakört.</span><span class="sxs-lookup"><span data-stu-id="02fe8-216">If you want to segment your users by interest groups, you can follow the [Azure Notification Hubs breaking news] tutorial.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
<span data-ttu-id="02fe8-217">[Chrome-alkalmazás – Notification Hub-minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps</span><span class="sxs-lookup"><span data-stu-id="02fe8-217">[Chrome App Notification Hub Sample]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps</span></span>
<span data-ttu-id="02fe8-218">[Google Cloud Console]: http://cloud.google.com/console</span><span class="sxs-lookup"><span data-stu-id="02fe8-218">[Google Cloud Console]: http://cloud.google.com/console</span></span>
[Azure Classic Portal]: https://manage.windowsazure.com/
<span data-ttu-id="02fe8-219">[Notification Hubs – áttekintés]: notification-hubs-push-notification-overview.md</span><span class="sxs-lookup"><span data-stu-id="02fe8-219">[Notification Hubs Overview]: notification-hubs-push-notification-overview.md</span></span>
<span data-ttu-id="02fe8-220">[Chrome-alkalmazások – áttekintés]: https://developer.chrome.com/apps/about_apps</span><span class="sxs-lookup"><span data-stu-id="02fe8-220">[Chrome Apps Overview]: https://developer.chrome.com/apps/about_apps</span></span>
<span data-ttu-id="02fe8-221">[Chrome-alkalmazások GCM mintája]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications</span><span class="sxs-lookup"><span data-stu-id="02fe8-221">[Chrome App GCM Sample]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications</span></span>
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
<span data-ttu-id="02fe8-222">[Chrome-alkalmazások mobileszközökön]: https://developer.chrome.com/apps/chrome_apps_on_mobile</span><span class="sxs-lookup"><span data-stu-id="02fe8-222">[Chrome Apps on Mobile]: https://developer.chrome.com/apps/chrome_apps_on_mobile</span></span>
<span data-ttu-id="02fe8-223">[Regisztrációs NH REST API létrehozása]: http://msdn.microsoft.com/library/azure/dn223265.aspx</span><span class="sxs-lookup"><span data-stu-id="02fe8-223">[Create Registration NH REST API]: http://msdn.microsoft.com/library/azure/dn223265.aspx</span></span>
<span data-ttu-id="02fe8-224">[crypto-js kódtárat]: http://code.google.com/p/crypto-js/</span><span class="sxs-lookup"><span data-stu-id="02fe8-224">[crypto-js library]: http://code.google.com/p/crypto-js/</span></span>
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
<span data-ttu-id="02fe8-225">[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1</span><span class="sxs-lookup"><span data-stu-id="02fe8-225">[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1</span></span>
<span data-ttu-id="02fe8-226">[Azure Notification Hubs – Felhasználók értesítése]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span><span class="sxs-lookup"><span data-stu-id="02fe8-226">[Azure Notification Hubs Notify Users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span></span>
<span data-ttu-id="02fe8-227">[Azure Notification Hubs – Legfrissebb hírek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span><span class="sxs-lookup"><span data-stu-id="02fe8-227">[Azure Notification Hubs breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span></span>
