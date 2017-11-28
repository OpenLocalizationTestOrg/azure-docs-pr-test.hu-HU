---
title: "aaaSend leküldéses értesítések tooChrome alkalmazások az Azure Notification hubs használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Chrome-alkalmazás."
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
ms.openlocfilehash: 7dec8ab02622563bc3730a2e96820da8932d22f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-push-notifications-toochrome-apps-with-azure-notification-hubs"></a><span data-ttu-id="4dce2-104">Leküldéses értesítések tooChrome alkalmazások az Azure Notification Hubs küldése</span><span class="sxs-lookup"><span data-stu-id="4dce2-104">Send push notifications tooChrome apps with Azure Notification Hubs</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

<span data-ttu-id="4dce2-105">Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Chrome-alkalmazás, amely hello kontextusában hello Google Chrome böngészőben jelenik.</span><span class="sxs-lookup"><span data-stu-id="4dce2-105">This topic shows you how toouse Azure Notification Hubs toosend push notifications tooa Chrome App, which will be displayed within hello context of hello Google Chrome browser.</span></span> <span data-ttu-id="4dce2-106">Ebben az oktatóanyagban létrehozunk egy olyan Chrome-alkalmazást, amely leküldéses értesítéseket fogad a [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/) használatával.</span><span class="sxs-lookup"><span data-stu-id="4dce2-106">In this tutorial, we will create a Chrome app that receives push notifications by using [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/).</span></span> 

> [!NOTE]
> <span data-ttu-id="4dce2-107">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="4dce2-107">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="4dce2-108">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="4dce2-108">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4dce2-109">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="4dce2-109">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="4dce2-110">hello oktatóanyag bemutatja, hogyan az alapvető lépéseken tooenable leküldéses értesítések:</span><span class="sxs-lookup"><span data-stu-id="4dce2-110">hello tutorial walks you through these basic steps tooenable push notifications:</span></span>

* [<span data-ttu-id="4dce2-111">A Google Cloud Messaging engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4dce2-111">Enable Google Cloud Messaging</span></span>](#register)
* [<span data-ttu-id="4dce2-112">Az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4dce2-112">Configure your notification hub</span></span>](#configure-hub)
* [<span data-ttu-id="4dce2-113">Csatlakozás a Chrome-alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="4dce2-113">Connect your Chrome App toohello notification hub</span></span>](#connect-app)
* [<span data-ttu-id="4dce2-114">A leküldéses értesítési tooyour Chrome-alkalmazás küldése</span><span class="sxs-lookup"><span data-stu-id="4dce2-114">Send a push notification tooyour Chrome App</span></span>](#send)
* [<span data-ttu-id="4dce2-115">További funkciók és képességek</span><span class="sxs-lookup"><span data-stu-id="4dce2-115">Additional functionality & capabilities</span></span>](#next-steps)

> [!NOTE]
> <span data-ttu-id="4dce2-116">Chrome-alkalmazások leküldéses értesítései nem általános böngészőn belüli értesítések - adott toohello böngésző bővíthetőségi modelljére (lásd: [Chrome-alkalmazások – áttekintés] részletekért).</span><span class="sxs-lookup"><span data-stu-id="4dce2-116">Chrome app push notifications are not generic in-browser notifications - they are specific toohello browser extensibility model (see [Chrome Apps Overview] for details).</span></span> <span data-ttu-id="4dce2-117">Ezenkívül toohello asztali böngészőn Chrome-alkalmazások mobileszközökön is futnak (Android és iOS) Apache Cordova segítségével.</span><span class="sxs-lookup"><span data-stu-id="4dce2-117">In addition toohello desktop browser, Chrome apps run on mobile (Android and iOS) through Apache Cordova.</span></span> <span data-ttu-id="4dce2-118">Lásd: [Chrome-alkalmazások mobileszközökön] további toolearn.</span><span class="sxs-lookup"><span data-stu-id="4dce2-118">See [Chrome Apps on Mobile] toolearn more.</span></span>
> 
> 

<span data-ttu-id="4dce2-119">Androidhoz készült azonos tooconfiguring azóta GCM és az Azure Notification Hubs [Google Cloud Messaging for Chrome] elavult, és ugyanaz a GCM hello mostantól támogatja az Android-eszközök és a Chrome-példányokat.</span><span class="sxs-lookup"><span data-stu-id="4dce2-119">Configuring GCM and Azure Notification Hubs is identical tooconfiguring for Android, since [Google Cloud Messaging for Chrome] has been deprecated and hello same GCM now supports both Android devices and Chrome instances.</span></span>

## <span data-ttu-id="4dce2-120"><a id="register"></a>A Google Cloud Messaging engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4dce2-120"><a id="register"></a>Enable Google Cloud Messaging</span></span>
1. <span data-ttu-id="4dce2-121">Keresse meg a toohello [Google Cloud Console] webhelyét, jelentkezzen be Google-fiók hitelesítő adataival, és kattintson a hello **projekt létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="4dce2-121">Navigate toohello [Google Cloud Console] website, sign in with your Google account credentials, and then click hello **Create Project** button.</span></span> <span data-ttu-id="4dce2-122">Adjon meg egy megfelelő **projektnevet**, majd kattintson a hello **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="4dce2-122">Provide an appropriate **Project Name**, and then click hello **Create** button.</span></span>
   
       ![Google Cloud Console - Create Project][1]
2. <span data-ttu-id="4dce2-123">Jegyezze fel a hello **Projektszám** a hello **projektek** lap hello projekthez létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4dce2-123">Make a note of hello **Project Number** on hello **Projects** page for hello project that you just created.</span></span> <span data-ttu-id="4dce2-124">Használni kívánt ez hello **GCM Sender ID** a hello Chrome-alkalmazás tooregister GCM-mel.</span><span class="sxs-lookup"><span data-stu-id="4dce2-124">You will use this as hello **GCM Sender ID** in hello Chrome App tooregister with GCM.</span></span>
   
       ![Google Cloud Console - Project Number][2]
3. <span data-ttu-id="4dce2-125">Hello bal oldali ablaktáblában kattintson **APIs & auth**, majd görgessen lefelé, majd kattintson a hello váltása tooenable **Google Cloud Messaging for Android**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-125">In hello left pane, click **APIs & auth**, and then scroll down and click hello toggle tooenable **Google Cloud Messaging for Android**.</span></span> <span data-ttu-id="4dce2-126">Nincs tooenable **Google Cloud Messaging for Chrome**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-126">You don't have tooenable **Google Cloud Messaging for Chrome**.</span></span>
   
       ![Google Cloud Console - Server Key][3]
4. <span data-ttu-id="4dce2-127">Hello bal oldali ablaktáblában kattintson **hitelesítő adatok** > **új kulcs létrehozása** > **Kiszolgálókulcs** > **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-127">In hello left pane, click **Credentials** > **Create New Key** > **Server Key** > **Create**.</span></span>
   
       ![Google Cloud Console - Credentials][4]
5. <span data-ttu-id="4dce2-128">Jegyezze fel a hello server **API-kulcs**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-128">Make a note of hello server **API Key**.</span></span> <span data-ttu-id="4dce2-129">A notification hub tovább, tooenable ez maguktól azt toosend leküldéses értesítések tooGCM.</span><span class="sxs-lookup"><span data-stu-id="4dce2-129">You will configure this in your notification hub next, tooenable it toosend push notifications tooGCM.</span></span>
   
       ![Google Cloud Console - API Key][5]

## <span data-ttu-id="4dce2-130"><a id="configure-hub"></a>A saját értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4dce2-130"><a id="configure-hub"></a>Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<span data-ttu-id="4dce2-131">&emsp;&emsp;6.</span><span class="sxs-lookup"><span data-stu-id="4dce2-131">&emsp;&emsp;6.</span></span>   <span data-ttu-id="4dce2-132">A hello **beállítások** panelen válassza **értesítési szolgáltatások** , majd **Google (GCM)**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-132">In hello **Settings** blade, select **Notification Services** and then **Google (GCM)**.</span></span> <span data-ttu-id="4dce2-133">Adja meg a hello API-kulcsot, majd mentse.</span><span class="sxs-lookup"><span data-stu-id="4dce2-133">Enter hello API key and save.</span></span>

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <span data-ttu-id="4dce2-135"><a id="connect-app"></a>Csatlakozás a Chrome-alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="4dce2-135"><a id="connect-app"></a>Connect your Chrome App toohello notification hub</span></span>
<span data-ttu-id="4dce2-136">Az értesítési központ már konfigurált toowork GCM-mel, és hello kapcsolati karakterláncok tooregister az alkalmazás tooboth leküldéses értesítések küldésére és fogadására van.</span><span class="sxs-lookup"><span data-stu-id="4dce2-136">Your notification hub is now configured toowork with GCM, and you have hello connection strings tooregister your app tooboth receive and send push notifications.</span></span> <span data-ttu-id="4dce2-137">LK</span><span class="sxs-lookup"><span data-stu-id="4dce2-137">LK</span></span>

### <a name="create-a-new-chrome-app"></a><span data-ttu-id="4dce2-138">Új Chrome-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4dce2-138">Create a new Chrome App</span></span>
<span data-ttu-id="4dce2-139">az alábbi hello minta alapján hello [Chrome-alkalmazások GCM mintája] és ajánlott módja toocreate a Chrome-alkalmazás által használt hello.</span><span class="sxs-lookup"><span data-stu-id="4dce2-139">hello sample below is based on hello [Chrome App GCM Sample] and uses hello recommended way toocreate a Chrome App.</span></span> <span data-ttu-id="4dce2-140">Hello lépéseket kifejezetten kapcsolódó tooAzure Notification Hubs ki.</span><span class="sxs-lookup"><span data-stu-id="4dce2-140">We will highlight hello steps specifically related tooAzure Notification Hubs.</span></span> 

> [!NOTE]
> <span data-ttu-id="4dce2-141">Azt javasoljuk, hogy a Chrome-alkalmazás a hello forrás letöltése [Chrome App Notification Hub-minta].</span><span class="sxs-lookup"><span data-stu-id="4dce2-141">We recommend that you download hello source for this Chrome App from [Chrome App Notification Hub Sample].</span></span>
> 
> 

<span data-ttu-id="4dce2-142">hello Chrome-alkalmazás JavaScripttel készült, és bármilyen szövegszerkesztővel létrehozható használhatja.</span><span class="sxs-lookup"><span data-stu-id="4dce2-142">hello Chrome App is created via JavaScript, and you can use any of your preferred word editors for creating it.</span></span> <span data-ttu-id="4dce2-143">A Chrome-alkalmazás a következőképpen fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="4dce2-143">Below is what this Chrome App will look like.</span></span>

![Google Chrome-alkalmazás][15]

1. <span data-ttu-id="4dce2-145">Hozzon létre egy `ChromePushApp` nevű mappát.</span><span class="sxs-lookup"><span data-stu-id="4dce2-145">Create a folder and name it `ChromePushApp`.</span></span> <span data-ttu-id="4dce2-146">Természetesen hello név tetszőleges – Ha Ön adjon neki nevet egy másik, győződjön meg arról, akkor helyettesítse be a szükséges hello kódrészleteket hello elérési utat.</span><span class="sxs-lookup"><span data-stu-id="4dce2-146">Of course, hello name is arbitrary - if you name it something different, make sure you substitute hello path in hello required code segments.</span></span>
2. <span data-ttu-id="4dce2-147">Töltse le a hello [crypto-js kódtárat] hello második lépésben létrehozott hello mappában.</span><span class="sxs-lookup"><span data-stu-id="4dce2-147">Download hello [crypto-js library] in hello folder you created in hello second step.</span></span> <span data-ttu-id="4dce2-148">Ez a kódtármappa a következő két almappát tartalmazza: `components` és `rollups`.</span><span class="sxs-lookup"><span data-stu-id="4dce2-148">This library folder will contain two subfolders: `components` and `rollups`.</span></span>
3. <span data-ttu-id="4dce2-149">Hozzon létre egy `manifest.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="4dce2-149">Create a `manifest.json` file.</span></span> <span data-ttu-id="4dce2-150">Az összes Chrome-alkalmazások üzemelnek, amely tartalmazza a hello alkalmazás metaadatait, és a legtöbb jegyzékfájlt is fontosabb, toohello app kapnak, amikor hello felhasználói telepítés összes engedélyt.</span><span class="sxs-lookup"><span data-stu-id="4dce2-150">All Chrome Apps are backed by a manifest file that contains hello app metadata and, most importantly, all permissions that are granted toohello app when hello user installs it.</span></span>
   
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
   
    <span data-ttu-id="4dce2-151">Értesítés hello `permissions` elemet, amely megadja, hogy a Chrome-alkalmazás képes tooreceive leküldéses értesítéseket a gcm-től.</span><span class="sxs-lookup"><span data-stu-id="4dce2-151">Notice hello `permissions` element, which specifies that this Chrome App will be able tooreceive push notifications from GCM.</span></span> <span data-ttu-id="4dce2-152">Azt is meg kell adnia hello Azure Notification Hubs URI ahol hello Chrome-alkalmazás egy REST-hívást tooregister biztosítják.</span><span class="sxs-lookup"><span data-stu-id="4dce2-152">It must also specify hello Azure Notification Hubs URI where hello Chrome App will make a REST call tooregister.</span></span>
    <span data-ttu-id="4dce2-153">Mintaalkalmazásunk a ikonfájlt is használja `gcm_128.png`, hogy az eredeti GCM-mintából hello hello forrásban talál.</span><span class="sxs-lookup"><span data-stu-id="4dce2-153">Our sample app also uses an icon file, `gcm_128.png`, that you will find at hello source that's reused from hello original GCM sample.</span></span> <span data-ttu-id="4dce2-154">Hello illő bármilyen képpel helyettesítheti [ikonokra vonatkozó követelményeknek](https://developer.chrome.com/apps/manifest/icons).</span><span class="sxs-lookup"><span data-stu-id="4dce2-154">You can substitute it for any image that fits hello [icon criteria](https://developer.chrome.com/apps/manifest/icons).</span></span>
4. <span data-ttu-id="4dce2-155">Hozzon létre egy nevű fájlt `background.js` a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="4dce2-155">Create a file called `background.js` with hello following code:</span></span>
   
        // Returns a new notification ID used in hello notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs tooform a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification tooshow hello GCM message.
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
   
        // Set up listeners tootrigger hello first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    <span data-ttu-id="4dce2-156">Hello fájl ugrik fel hello Chrome alkalmazás HTML-ablakában (**register.html**) és is definiálja hello kezelő **messageReceived** toohandle hello bejövő leküldéses értesítést.</span><span class="sxs-lookup"><span data-stu-id="4dce2-156">This is hello file that pops up hello Chrome App window HTML (**register.html**) and also defines hello handler **messageReceived** toohandle hello incoming push notification.</span></span>
5. <span data-ttu-id="4dce2-157">Hozzon létre egy nevű fájlt `register.html` -hello hello Chrome-alkalmazás felhasználói Felületét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="4dce2-157">Create a file called `register.html` - this defines hello UI of hello Chrome App.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="4dce2-158">Ebben a példában a következőt használjuk: **CryptoJS v3.1.2**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-158">This sample uses **CryptoJS v3.1.2**.</span></span> <span data-ttu-id="4dce2-159">Ha hello kódtár másik verzióját töltötte le, ellenőrizze, hogy megfelelően helyettesítse hello hello verzióját `src` elérési útja.</span><span class="sxs-lookup"><span data-stu-id="4dce2-159">If you downloaded another version of hello library, make sure you properly substitute hello version in hello `src` path.</span></span>
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
6. <span data-ttu-id="4dce2-160">Hozzon létre egy nevű fájlt `register.js` az alábbi hello kóddal.</span><span class="sxs-lookup"><span data-stu-id="4dce2-160">Create a file called `register.js` with hello code below.</span></span> <span data-ttu-id="4dce2-161">Ez a fájl hello szkriptjét határozza meg `register.html`.</span><span class="sxs-lookup"><span data-stu-id="4dce2-161">This file specifies hello script behind `register.html`.</span></span> <span data-ttu-id="4dce2-162">Chrome-alkalmazások nem engedélyezett beágyazott Futtatás használatát, így kell toocreate külön háttérszkriptet a felhasználói felülethez.</span><span class="sxs-lookup"><span data-stu-id="4dce2-162">Chrome Apps do not allow inline execution, so you have toocreate a separate backing script for your UI.</span></span>
   
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
   
          // Prevent register button from being clicked again before hello registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When hello registration fails, handle hello error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that hello first-time registration is done.
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
   
          // Update hello payload with hello registration ID obtained earlier.
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
   
    <span data-ttu-id="4dce2-163">parancsfájl fent hello rendelkezik hello a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="4dce2-163">hello above script has hello following key parameters:</span></span>
   
   * <span data-ttu-id="4dce2-164">**window.onload** hello eseményeit hello két gombok definiál hello felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="4dce2-164">**window.onload** defines hello button-click events of hello two buttons on hello UI.</span></span> <span data-ttu-id="4dce2-165">Egyik a GCM-ben regisztrál, és más hello hello GCM tooregister az Azure Notification hubs használatával történő regisztráció után visszaadott regisztrációs Azonosítót használja.</span><span class="sxs-lookup"><span data-stu-id="4dce2-165">One registers with GCM, and hello other uses hello registration ID that's returned after registration with GCM tooregister with Azure Notification Hubs.</span></span>
   * <span data-ttu-id="4dce2-166">**updateLog** hello funkciója, amely lehetővé teszi toohandle egyszerű naplózási képességeket.</span><span class="sxs-lookup"><span data-stu-id="4dce2-166">**updateLog** is hello function that allows us toohandle simple logging capabilities.</span></span>
   * <span data-ttu-id="4dce2-167">**registerWithGCM** hello első kattintáskezelő, amely lehetővé teszi a hello van `chrome.gcm.register` hívás tooGCM tooregister hello aktuális Chrome App-példány.</span><span class="sxs-lookup"><span data-stu-id="4dce2-167">**registerWithGCM** is hello first button-click handler, which makes hello `chrome.gcm.register` call tooGCM tooregister hello current Chrome App instance.</span></span>
   * <span data-ttu-id="4dce2-168">**registerCallback** hello visszahívási függvény meghívása megtörténik, amikor hello GCM-regisztrációs hívás adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4dce2-168">**registerCallback** is hello callback function that gets called when hello GCM registration call returns.</span></span>
   * <span data-ttu-id="4dce2-169">**registerWithNH** hello második kattintáskezelő, amely a Notification Hubs van.</span><span class="sxs-lookup"><span data-stu-id="4dce2-169">**registerWithNH** is hello second button-click handler, which registers with Notification Hubs.</span></span> <span data-ttu-id="4dce2-170">Lekéri `hubName` és `connectionString` (mely hello felhasználó által megadott) és kézműipari hello Notification Hubs regisztrációs REST API-hívás.</span><span class="sxs-lookup"><span data-stu-id="4dce2-170">It gets `hubName` and `connectionString` (which hello user has specified) and crafts hello Notification Hubs Registration REST API call.</span></span>
   * <span data-ttu-id="4dce2-171">**splitConnectionString** és **generateSaSToken** olyan segítők, amelyek megfelelnek egy SaS-token létrehozási folyamata, minden REST API-hívásokban használandó hello JavaScript végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="4dce2-171">**splitConnectionString** and **generateSaSToken** are helpers that represent hello JavaScript implementation of a SaS token creation process, that must be used in all REST API calls.</span></span> <span data-ttu-id="4dce2-172">További információk: [Általánosan használt fogalmak](http://msdn.microsoft.com/library/dn495627.aspx).</span><span class="sxs-lookup"><span data-stu-id="4dce2-172">For more information, see [Common Concepts](http://msdn.microsoft.com/library/dn495627.aspx).</span></span>
   * <span data-ttu-id="4dce2-173">**sendNHRegistrationRequest** van egy HTTP REST hello függvény hívása tooAzure Notification hubs használatával.</span><span class="sxs-lookup"><span data-stu-id="4dce2-173">**sendNHRegistrationRequest** is hello function that makes a HTTP REST call tooAzure Notification Hubs.</span></span>
   * <span data-ttu-id="4dce2-174">**registrationPayload** hello regisztrációs XML hasznos adatait definiálja.</span><span class="sxs-lookup"><span data-stu-id="4dce2-174">**registrationPayload** defines hello registration XML payload.</span></span> <span data-ttu-id="4dce2-175">További információk: [Regisztrációs NH REST API létrehozása].</span><span class="sxs-lookup"><span data-stu-id="4dce2-175">For more information, see [Create Registration NH REST API].</span></span> <span data-ttu-id="4dce2-176">Hello benne található regisztrációs Azonosítót kapott adatokkal frissítjük a gcm-től frissítjük.</span><span class="sxs-lookup"><span data-stu-id="4dce2-176">We update hello registration ID in it with what we received from GCM.</span></span>
   * <span data-ttu-id="4dce2-177">**ügyfél** példánya **XMLHttpRequest** toomake hello HTTP POST-kérelmet használjuk.</span><span class="sxs-lookup"><span data-stu-id="4dce2-177">**client** is an instance of **XMLHttpRequest** that we use toomake hello HTTP POST request.</span></span> <span data-ttu-id="4dce2-178">Vegye figyelembe, hogy a Microsoft update hello `Authorization` fejléc a következő `sasToken`.</span><span class="sxs-lookup"><span data-stu-id="4dce2-178">Note that we update hello `Authorization` header with `sasToken`.</span></span> <span data-ttu-id="4dce2-179">A hívás sikeres végrehajtása regisztrálja a Chrome-alkalmazás ezen példányát az Azure Notification Hubsban.</span><span class="sxs-lookup"><span data-stu-id="4dce2-179">Successful completion of this call will register this Chrome App instance with Azure Notification Hubs.</span></span>

<span data-ttu-id="4dce2-180">hello a projekt teljes mappastruktúrája kell a következőhöz hasonló lesz: ![Google Chrome-alkalmazás – mappastruktúra][21]</span><span class="sxs-lookup"><span data-stu-id="4dce2-180">hello overall folder structure for this project should resemble this: ![Google Chrome App - Folder Structure][21]</span></span>

### <a name="set-up-and-test-your-chrome-app"></a><span data-ttu-id="4dce2-181">A Chrome-alkalmazás beállítása és tesztelése</span><span class="sxs-lookup"><span data-stu-id="4dce2-181">Set up and test your Chrome App</span></span>
1. <span data-ttu-id="4dce2-182">Nyissa meg a Chrome böngészőt.</span><span class="sxs-lookup"><span data-stu-id="4dce2-182">Open your Chrome browser.</span></span> <span data-ttu-id="4dce2-183">Nyissa meg a **Chrome-bővítményeket**, majd engedélyezze a **Fejlesztői módot**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-183">Open **Chrome extensions** and enable **Developer mode**.</span></span>
   
       ![Google Chrome - Enable Developer Mode][16]
2. <span data-ttu-id="4dce2-184">Kattintson a **kicsomagolatlan bővítmény betöltése** , és keresse meg a toohello mappa, amelyben létrehozta hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="4dce2-184">Click **Load unpacked extension** and navigate toohello folder where you created hello files.</span></span> <span data-ttu-id="4dce2-185">Is használhatja a hello **Chrome-alkalmazások és bővítmények fejlesztői eszközét**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-185">You can also optionally use hello **Chrome Apps & Extensions Developer Tool**.</span></span> <span data-ttu-id="4dce2-186">Ez az eszköz a Chrome-alkalmazás önmagában (telepített hello Chrome webes tároló), és speciális hibakeresési képességeket biztosít a Chrome-alkalmazások fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="4dce2-186">This tool is a Chrome App in itself (installed from hello Chrome Web Store) and provides advanced debugging capabilities for your Chrome App development.</span></span>
   
       ![Google Chrome - Load Unpacked Extension][17]
3. <span data-ttu-id="4dce2-187">Ha hello Chrome-alkalmazás létrehozása hibaüzenet nélkül van, majd látni fogja a Chrome-alkalmazás jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4dce2-187">If hello Chrome App is created without any errors, then you will see your Chrome App show up.</span></span>
   
       ![Google Chrome - Chrome App Display][18]
4. <span data-ttu-id="4dce2-188">Adja meg a hello **Projektszám** , hogy ból korábban beszerzett hello **Google Cloud Console** elemet hello Küldőazonosító, és kattintson **a GCM-regisztrációs**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-188">Enter hello **Project Number** that you got earlier from hello **Google Cloud Console** as hello sender ID, and click **Register with GCM**.</span></span> <span data-ttu-id="4dce2-189">Hello üzenetet kell látnia: **regisztráció sikeres GCM-mel.**</span><span class="sxs-lookup"><span data-stu-id="4dce2-189">You must see hello message **Registration with GCM succeeded.**</span></span>
   
       ![Google Chrome - Chrome App Customization][19]
5. <span data-ttu-id="4dce2-190">Adja meg a **az értesítési központ neve** és hello **DefaultListenSharedAccessSignature** korábbi hello portálra, majd kattintson a beszerzett **Azure Notification Hubregisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-190">Enter your **Notification Hub Name** and hello **DefaultListenSharedAccessSignature** that you obtained from hello portal earlier, and click **Register with Azure Notification Hub**.</span></span> <span data-ttu-id="4dce2-191">Hello üzenetet kell látnia: **sikeres Notification Hub regisztrációs!**</span><span class="sxs-lookup"><span data-stu-id="4dce2-191">You must see hello message **Notification Hub Registration successful!**</span></span> <span data-ttu-id="4dce2-192">és hello regisztrációs választ, amely tartalmazza az Azure Notification Hubs-regisztráció hello hello részleteinek-azonosító.</span><span class="sxs-lookup"><span data-stu-id="4dce2-192">and hello details of hello registration response, which contains hello Azure Notification Hubs registration ID.</span></span>
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <span data-ttu-id="4dce2-193"><a name="send"></a>Elküldeni egy értesítést tooyour Chrome-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="4dce2-193"><a name="send"></a>Send a notification tooyour Chrome App</span></span>
<span data-ttu-id="4dce2-194">Tesztelési célra a Chrome leküldéses értesítéseit egy .NET-konzolalkalmazás használatával küldjük el.</span><span class="sxs-lookup"><span data-stu-id="4dce2-194">For testing purposes, we will send Chrome push notifications by using a .NET console application.</span></span> 

> [!NOTE]
> <span data-ttu-id="4dce2-195">A Notification Hubs használatával bármilyen háttérrendszerből küldhet leküldéses értesítést a nyilvános <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-felületen</a> keresztül.</span><span class="sxs-lookup"><span data-stu-id="4dce2-195">You can send push notifications with Notification Hubs from any backend via our public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="4dce2-196">A platformok közötti átjárhatóságról további példákat a [dokumentációs portálon](https://azure.microsoft.com/documentation/services/notification-hubs/) talál.</span><span class="sxs-lookup"><span data-stu-id="4dce2-196">Check out our [documentation portal](https://azure.microsoft.com/documentation/services/notification-hubs/) for more cross-platform examples.</span></span>
> 
> 

1. <span data-ttu-id="4dce2-197">A Visual Studio, a hello **fájl** menü **új** , majd **projekt**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-197">In Visual Studio, from hello **File** menu, select **New** and then **Project**.</span></span> <span data-ttu-id="4dce2-198">A **Visual C#** területen kattintson a **Windows** és a **Konzolalkalmazás**, majd pedig az **OK** elemre.</span><span class="sxs-lookup"><span data-stu-id="4dce2-198">Under **Visual C#**, click **Windows** and **Console Application**, and then click **OK**.</span></span>  <span data-ttu-id="4dce2-199">Ekkor létrejön egy új konzolalkalmazás-projekt.</span><span class="sxs-lookup"><span data-stu-id="4dce2-199">This creates a new console application project.</span></span>
2. <span data-ttu-id="4dce2-200">A hello **eszközök** menüben kattintson a **Kódtárcsomag-kezelő** , majd **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="4dce2-200">From hello **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span> <span data-ttu-id="4dce2-201">Ekkor megjelenik a Package Manager Console hello.</span><span class="sxs-lookup"><span data-stu-id="4dce2-201">This displays hello Package Manager Console.</span></span>
3. <span data-ttu-id="4dce2-202">Hello konzolablakban hajtható végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4dce2-202">In hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference toohello Azure Service Bus SDK with hello <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. <span data-ttu-id="4dce2-203">Nyissa meg `Program.cs` és adja hozzá a következő hello `using` utasítást:</span><span class="sxs-lookup"><span data-stu-id="4dce2-203">Open `Program.cs` and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="4dce2-204">A hello `Program` osztály, adja hozzá a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="4dce2-204">In hello `Program` class, add hello following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace hello connection string placeholder with hello connection string called `DefaultFullSharedAccessSignature` that you obtained in hello notification hub configuration section.
   
   > [!NOTE]
   > <span data-ttu-id="4dce2-205">Győződjön meg arról, hogy használja-e hello kapcsolati karakterlánc **teljes** fér hozzá, nem **figyelésére** hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="4dce2-205">Make sure that you use hello connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="4dce2-206">Hello **figyelésére** hozzáféréssel rendelkező kapcsolati karakterláncok nem biztosít engedélyeket toosend leküldéses értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="4dce2-206">hello **Listen** access connection string does not grant permissions toosend push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="4dce2-207">Adja hozzá a hello következő hívások hello `Main` módszert:</span><span class="sxs-lookup"><span data-stu-id="4dce2-207">Add hello following calls in hello `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="4dce2-208">Győződjön meg arról, hogy a Chrome fut-e, és futtassa a konzolalkalmazást hello.</span><span class="sxs-lookup"><span data-stu-id="4dce2-208">Make sure that Chrome is running, and run hello console application.</span></span>
8. <span data-ttu-id="4dce2-209">Hello következő kell megjelennie az asztalon felugró értesítést.</span><span class="sxs-lookup"><span data-stu-id="4dce2-209">You should see hello following notification pop up on your desktop.</span></span>
   
       ![Google Chrome - Notification][13]
9. <span data-ttu-id="4dce2-210">Azt is láthatja, az értesítések használatával hello tálca Chrome-értesítések ablakában hello (Windows) Ha a Chrome fut.</span><span class="sxs-lookup"><span data-stu-id="4dce2-210">You can also see all your notifications by using hello Chrome Notifications window in hello taskbar (in Windows) when Chrome is running.</span></span>
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> <span data-ttu-id="4dce2-211">Nincs szükség a toohave hello Chrome-alkalmazás fut, vagy nyissa meg böngészőben hello (bár a hello Chrome böngészőnek futnia kell).</span><span class="sxs-lookup"><span data-stu-id="4dce2-211">You don't need toohave hello Chrome App running or open in hello browser (though hello Chrome browser itself must be running).</span></span> <span data-ttu-id="4dce2-212">Hello Chrome-értesítések ablakban az összes értesítés egyesített nézetét is kap.</span><span class="sxs-lookup"><span data-stu-id="4dce2-212">You also get a consolidated view of all your notifications in hello Chrome Notifications window.</span></span>
> 
> 

## <span data-ttu-id="4dce2-213"><a name="next-steps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4dce2-213"><a name="next-steps"> </a>Next steps</span></span>
<span data-ttu-id="4dce2-214">További tudnivalók a Notification Hubsról: [Notification Hubs – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="4dce2-214">Learn more about Notification Hubs in [Notification Hubs Overview].</span></span>

<span data-ttu-id="4dce2-215">adott felhasználók tootarget, tekintse meg a toohello [Azure Notification Hubs – felhasználók értesítése] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4dce2-215">tootarget specific users, refer toohello [Azure Notification Hubs Notify Users] tutorial.</span></span> 

<span data-ttu-id="4dce2-216">Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, követésével hello [Azure Notification Hubs – legfrissebb hírek] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4dce2-216">If you want toosegment your users by interest groups, you can follow hello [Azure Notification Hubs breaking news] tutorial.</span></span>

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
[Chrome App Notification Hub-minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud Console]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs – áttekintés]: notification-hubs-push-notification-overview.md
[Chrome-alkalmazások – áttekintés]: https://developer.chrome.com/apps/about_apps
[Chrome-alkalmazások GCM mintája]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-alkalmazások mobileszközökön]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Regisztrációs NH REST API létrehozása]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[crypto-js kódtárat]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs – felhasználók értesítése]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs – legfrissebb hírek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
