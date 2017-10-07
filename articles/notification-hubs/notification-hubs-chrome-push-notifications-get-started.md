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
# <a name="send-push-notifications-toochrome-apps-with-azure-notification-hubs"></a>Leküldéses értesítések tooChrome alkalmazások az Azure Notification Hubs küldése
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toosend leküldéses értesítések tooa Chrome-alkalmazás, amely hello kontextusában hello Google Chrome böngészőben jelenik. Ebben az oktatóanyagban létrehozunk egy olyan Chrome-alkalmazást, amely leküldéses értesítéseket fogad a [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/) használatával. 

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).
> 
> 

hello oktatóanyag bemutatja, hogyan az alapvető lépéseken tooenable leküldéses értesítések:

* [A Google Cloud Messaging engedélyezése](#register)
* [Az értesítési központ konfigurálása](#configure-hub)
* [Csatlakozás a Chrome-alkalmazás toohello értesítési központ](#connect-app)
* [A leküldéses értesítési tooyour Chrome-alkalmazás küldése](#send)
* [További funkciók és képességek](#next-steps)

> [!NOTE]
> Chrome-alkalmazások leküldéses értesítései nem általános böngészőn belüli értesítések - adott toohello böngésző bővíthetőségi modelljére (lásd: [Chrome-alkalmazások – áttekintés] részletekért). Ezenkívül toohello asztali böngészőn Chrome-alkalmazások mobileszközökön is futnak (Android és iOS) Apache Cordova segítségével. Lásd: [Chrome-alkalmazások mobileszközökön] további toolearn.
> 
> 

Androidhoz készült azonos tooconfiguring azóta GCM és az Azure Notification Hubs [Google Cloud Messaging for Chrome] elavult, és ugyanaz a GCM hello mostantól támogatja az Android-eszközök és a Chrome-példányokat.

## <a id="register"></a>A Google Cloud Messaging engedélyezése
1. Keresse meg a toohello [Google Cloud Console] webhelyét, jelentkezzen be Google-fiók hitelesítő adataival, és kattintson a hello **projekt létrehozása** gombra. Adjon meg egy megfelelő **projektnevet**, majd kattintson a hello **létrehozása** gombra.
   
       ![Google Cloud Console - Create Project][1]
2. Jegyezze fel a hello **Projektszám** a hello **projektek** lap hello projekthez létrehozott. Használni kívánt ez hello **GCM Sender ID** a hello Chrome-alkalmazás tooregister GCM-mel.
   
       ![Google Cloud Console - Project Number][2]
3. Hello bal oldali ablaktáblában kattintson **APIs & auth**, majd görgessen lefelé, majd kattintson a hello váltása tooenable **Google Cloud Messaging for Android**. Nincs tooenable **Google Cloud Messaging for Chrome**.
   
       ![Google Cloud Console - Server Key][3]
4. Hello bal oldali ablaktáblában kattintson **hitelesítő adatok** > **új kulcs létrehozása** > **Kiszolgálókulcs** > **létrehozása**.
   
       ![Google Cloud Console - Credentials][4]
5. Jegyezze fel a hello server **API-kulcs**. A notification hub tovább, tooenable ez maguktól azt toosend leküldéses értesítések tooGCM.
   
       ![Google Cloud Console - API Key][5]

## <a id="configure-hub"></a>A saját értesítési központ konfigurálása
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6.   A hello **beállítások** panelen válassza **értesítési szolgáltatások** , majd **Google (GCM)**. Adja meg a hello API-kulcsot, majd mentse.

&emsp;&emsp;![Azure Notification Hubs – Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

## <a id="connect-app"></a>Csatlakozás a Chrome-alkalmazás toohello értesítési központ
Az értesítési központ már konfigurált toowork GCM-mel, és hello kapcsolati karakterláncok tooregister az alkalmazás tooboth leküldéses értesítések küldésére és fogadására van. LK

### <a name="create-a-new-chrome-app"></a>Új Chrome-alkalmazás létrehozása
az alábbi hello minta alapján hello [Chrome-alkalmazások GCM mintája] és ajánlott módja toocreate a Chrome-alkalmazás által használt hello. Hello lépéseket kifejezetten kapcsolódó tooAzure Notification Hubs ki. 

> [!NOTE]
> Azt javasoljuk, hogy a Chrome-alkalmazás a hello forrás letöltése [Chrome App Notification Hub-minta].
> 
> 

hello Chrome-alkalmazás JavaScripttel készült, és bármilyen szövegszerkesztővel létrehozható használhatja. A Chrome-alkalmazás a következőképpen fog kinézni.

![Google Chrome-alkalmazás][15]

1. Hozzon létre egy `ChromePushApp` nevű mappát. Természetesen hello név tetszőleges – Ha Ön adjon neki nevet egy másik, győződjön meg arról, akkor helyettesítse be a szükséges hello kódrészleteket hello elérési utat.
2. Töltse le a hello [crypto-js kódtárat] hello második lépésben létrehozott hello mappában. Ez a kódtármappa a következő két almappát tartalmazza: `components` és `rollups`.
3. Hozzon létre egy `manifest.json` fájlt. Az összes Chrome-alkalmazások üzemelnek, amely tartalmazza a hello alkalmazás metaadatait, és a legtöbb jegyzékfájlt is fontosabb, toohello app kapnak, amikor hello felhasználói telepítés összes engedélyt.
   
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
   
    Értesítés hello `permissions` elemet, amely megadja, hogy a Chrome-alkalmazás képes tooreceive leküldéses értesítéseket a gcm-től. Azt is meg kell adnia hello Azure Notification Hubs URI ahol hello Chrome-alkalmazás egy REST-hívást tooregister biztosítják.
    Mintaalkalmazásunk a ikonfájlt is használja `gcm_128.png`, hogy az eredeti GCM-mintából hello hello forrásban talál. Hello illő bármilyen képpel helyettesítheti [ikonokra vonatkozó követelményeknek](https://developer.chrome.com/apps/manifest/icons).
4. Hozzon létre egy nevű fájlt `background.js` a hello a következő kódot:
   
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
   
    Hello fájl ugrik fel hello Chrome alkalmazás HTML-ablakában (**register.html**) és is definiálja hello kezelő **messageReceived** toohandle hello bejövő leküldéses értesítést.
5. Hozzon létre egy nevű fájlt `register.html` -hello hello Chrome-alkalmazás felhasználói Felületét határozza meg. 
   
   > [!NOTE]
   > Ebben a példában a következőt használjuk: **CryptoJS v3.1.2**. Ha hello kódtár másik verzióját töltötte le, ellenőrizze, hogy megfelelően helyettesítse hello hello verzióját `src` elérési útja.
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
6. Hozzon létre egy nevű fájlt `register.js` az alábbi hello kóddal. Ez a fájl hello szkriptjét határozza meg `register.html`. Chrome-alkalmazások nem engedélyezett beágyazott Futtatás használatát, így kell toocreate külön háttérszkriptet a felhasználói felülethez.
   
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
   
    parancsfájl fent hello rendelkezik hello a következő paramétereket:
   
   * **window.onload** hello eseményeit hello két gombok definiál hello felhasználói felületén. Egyik a GCM-ben regisztrál, és más hello hello GCM tooregister az Azure Notification hubs használatával történő regisztráció után visszaadott regisztrációs Azonosítót használja.
   * **updateLog** hello funkciója, amely lehetővé teszi toohandle egyszerű naplózási képességeket.
   * **registerWithGCM** hello első kattintáskezelő, amely lehetővé teszi a hello van `chrome.gcm.register` hívás tooGCM tooregister hello aktuális Chrome App-példány.
   * **registerCallback** hello visszahívási függvény meghívása megtörténik, amikor hello GCM-regisztrációs hívás adja vissza.
   * **registerWithNH** hello második kattintáskezelő, amely a Notification Hubs van. Lekéri `hubName` és `connectionString` (mely hello felhasználó által megadott) és kézműipari hello Notification Hubs regisztrációs REST API-hívás.
   * **splitConnectionString** és **generateSaSToken** olyan segítők, amelyek megfelelnek egy SaS-token létrehozási folyamata, minden REST API-hívásokban használandó hello JavaScript végrehajtására. További információk: [Általánosan használt fogalmak](http://msdn.microsoft.com/library/dn495627.aspx).
   * **sendNHRegistrationRequest** van egy HTTP REST hello függvény hívása tooAzure Notification hubs használatával.
   * **registrationPayload** hello regisztrációs XML hasznos adatait definiálja. További információk: [Regisztrációs NH REST API létrehozása]. Hello benne található regisztrációs Azonosítót kapott adatokkal frissítjük a gcm-től frissítjük.
   * **ügyfél** példánya **XMLHttpRequest** toomake hello HTTP POST-kérelmet használjuk. Vegye figyelembe, hogy a Microsoft update hello `Authorization` fejléc a következő `sasToken`. A hívás sikeres végrehajtása regisztrálja a Chrome-alkalmazás ezen példányát az Azure Notification Hubsban.

hello a projekt teljes mappastruktúrája kell a következőhöz hasonló lesz: ![Google Chrome-alkalmazás – mappastruktúra][21]

### <a name="set-up-and-test-your-chrome-app"></a>A Chrome-alkalmazás beállítása és tesztelése
1. Nyissa meg a Chrome böngészőt. Nyissa meg a **Chrome-bővítményeket**, majd engedélyezze a **Fejlesztői módot**.
   
       ![Google Chrome - Enable Developer Mode][16]
2. Kattintson a **kicsomagolatlan bővítmény betöltése** , és keresse meg a toohello mappa, amelyben létrehozta hello fájlokat. Is használhatja a hello **Chrome-alkalmazások és bővítmények fejlesztői eszközét**. Ez az eszköz a Chrome-alkalmazás önmagában (telepített hello Chrome webes tároló), és speciális hibakeresési képességeket biztosít a Chrome-alkalmazások fejlesztéséhez.
   
       ![Google Chrome - Load Unpacked Extension][17]
3. Ha hello Chrome-alkalmazás létrehozása hibaüzenet nélkül van, majd látni fogja a Chrome-alkalmazás jelenik meg.
   
       ![Google Chrome - Chrome App Display][18]
4. Adja meg a hello **Projektszám** , hogy ból korábban beszerzett hello **Google Cloud Console** elemet hello Küldőazonosító, és kattintson **a GCM-regisztrációs**. Hello üzenetet kell látnia: **regisztráció sikeres GCM-mel.**
   
       ![Google Chrome - Chrome App Customization][19]
5. Adja meg a **az értesítési központ neve** és hello **DefaultListenSharedAccessSignature** korábbi hello portálra, majd kattintson a beszerzett **Azure Notification Hubregisztrálása**. Hello üzenetet kell látnia: **sikeres Notification Hub regisztrációs!** és hello regisztrációs választ, amely tartalmazza az Azure Notification Hubs-regisztráció hello hello részleteinek-azonosító.
   
       ![Google Chrome - Specify Notification Hub Details][20]  

## <a name="send"></a>Elküldeni egy értesítést tooyour Chrome-alkalmazás
Tesztelési célra a Chrome leküldéses értesítéseit egy .NET-konzolalkalmazás használatával küldjük el. 

> [!NOTE]
> A Notification Hubs használatával bármilyen háttérrendszerből küldhet leküldéses értesítést a nyilvános <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-felületen</a> keresztül. A platformok közötti átjárhatóságról további példákat a [dokumentációs portálon](https://azure.microsoft.com/documentation/services/notification-hubs/) talál.
> 
> 

1. A Visual Studio, a hello **fájl** menü **új** , majd **projekt**. A **Visual C#** területen kattintson a **Windows** és a **Konzolalkalmazás**, majd pedig az **OK** elemre.  Ekkor létrejön egy új konzolalkalmazás-projekt.
2. A hello **eszközök** menüben kattintson a **Kódtárcsomag-kezelő** , majd **Csomagkezelő konzol**. Ekkor megjelenik a Package Manager Console hello.
3. Hello konzolablakban hajtható végre a következő parancs hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
       This adds a reference toohello Azure Service Bus SDK with hello <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>.
4. Nyissa meg `Program.cs` és adja hozzá a következő hello `using` utasítást:
   
        using Microsoft.Azure.NotificationHubs;
5. A hello `Program` osztály, adja hozzá a következő metódus hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
       Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello [portal](https://portal.azure.com) in your Notification Hub blade. Also, replace hello connection string placeholder with hello connection string called `DefaultFullSharedAccessSignature` that you obtained in hello notification hub configuration section.
   
   > [!NOTE]
   > Győződjön meg arról, hogy használja-e hello kapcsolati karakterlánc **teljes** fér hozzá, nem **figyelésére** hozzáférést. Hello **figyelésére** hozzáféréssel rendelkező kapcsolati karakterláncok nem biztosít engedélyeket toosend leküldéses értesítéseket.
   > 
   > 
6. Adja hozzá a hello következő hívások hello `Main` módszert:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Győződjön meg arról, hogy a Chrome fut-e, és futtassa a konzolalkalmazást hello.
8. Hello következő kell megjelennie az asztalon felugró értesítést.
   
       ![Google Chrome - Notification][13]
9. Azt is láthatja, az értesítések használatával hello tálca Chrome-értesítések ablakában hello (Windows) Ha a Chrome fut.
   
       ![Google Chrome - Notifications List][14]

> [!NOTE]
> Nincs szükség a toohave hello Chrome-alkalmazás fut, vagy nyissa meg böngészőben hello (bár a hello Chrome böngészőnek futnia kell). Hello Chrome-értesítések ablakban az összes értesítés egyesített nézetét is kap.
> 
> 

## <a name="next-steps"></a>Következő lépések
További tudnivalók a Notification Hubsról: [Notification Hubs – áttekintés].

adott felhasználók tootarget, tekintse meg a toohello [Azure Notification Hubs – felhasználók értesítése] oktatóanyag. 

Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, követésével hello [Azure Notification Hubs – legfrissebb hírek] oktatóanyag.

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
