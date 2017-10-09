---
title: "az Azure Notification Hubs és a Node.js aaaSending leküldéses értesítések"
description: "Tudnivalók a Notification Hubs toosend toouse a leküldéses értesítések Node.js-alkalmazás."
keywords: "leküldéses értesítési, leküldéses notifications,node.js leküldéses, ios leküldéses"
services: notification-hubs
documentationcenter: nodejs
author: ysxu
manager: erikre
editor: 
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/25/2016
ms.author: yuaxu
ms.openlocfilehash: 151d224fa6dd07e4acdc3a4887c4e95ee03168c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Leküldéses értesítések küldése az Azure Notification Hubs és a Node.js
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a>Áttekintés
> [!IMPORTANT]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).
> 
> 

Ez az útmutató bemutatja, mindez toosend a leküldéses értesítések hello segítségével az Azure Notification Hubs közvetlenül a Node.js-alkalmazás. 

hello jelzett esetek tooapplications a leküldéses értesítések küldése a következő platformokon hello:

* Android
* iOS
* Windows Phone
* Az univerzális Windows Platform 

Notification hubs használatával kapcsolatban lásd: hello [lépések](#next) szakasz.

## <a name="what-are-notification-hubs"></a>Mi a Notification Hubs szolgáltatás?
Az Azure Notification Hubs egy egyszerűen használható, többplatformos, méretezhető infrastruktúrát biztosít a leküldéses értesítések toomobile eszközök. Hello szolgáltatás-infrastruktúrát a részletekért lásd: hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) lap.

## <a name="create-a-nodejs-application"></a>Node.js-alkalmazás létrehozása
hello első lépés az oktatóanyag egy új, üres Node.js-alkalmazás létrehozása. A Node.js-alkalmazás létrehozása, lásd: [létrehozása és központi telepítése egy Node.js-alkalmazás tooAzure webhely][nodejswebsite], [Node.js Felhőszolgáltatás] [ Node.js Cloud Service] Windows PowerShell használatával vagy [webhelyet a WebMatrix].

## <a name="configure-your-application-toouse-notification-hubs"></a>Alkalmazás tooUse Notification Hubs konfigurálása
Azure Notification Hubs toouse, és van szükség toodownload használata hello Node.js [azure-csomag](https://www.npmjs.com/package/azure), amely tartalmaz egy beépített hello leküldéses értesítési REST szolgáltatásokkal kommunikálni segítő szalagtárak.

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a>Csomópont Package Manager (NPM) tooobtain hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Linux), és keresse meg a toohello mappa, amelyben létrehozta az üres alkalmazás .
2. Típus **npm telepítése azure-sb** hello parancssori ablakban.
3. Manuális futtatásával hello **ls** vagy **dir** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre. A mappában található hello **azure** csomagot, amely tartalmaz hello szalagtárak tooaccess kell hello értesítési központot.

> [!NOTE]
> További hello hivatalos NPM telepítéséről [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 
> 
> 

### <a name="import-hello-module"></a>Importálás hello modul
Egy szövegszerkesztővel, adja hozzá a következő toohello felső részén hello hello **server.js** hello alkalmazás fájlt:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Az Azure Notification Hub-kapcsolat beállítása
Hello **NotificationHubService** objektum lehetővé teszi, hogy a notification hubs használatával. hello alábbi kód létrehoz egy **NotificationHubService** hello nofication központ nevű objektum **hubname**. Hello hello tetején adja hozzá **server.js** fájl hello utasítás tooimport hello azure modul után:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

kapcsolat hello **connectionstring** érték hello szerezhető [Azure Portal] hello lépések végrehajtásával:

1. Hello bal oldali navigációs ablaktábláján kattintson **Tallózás**.
2. Válassza ki **Notification Hubs**, és majd keresés hello hub hello minta toouse kívánja. Olvassa el a toohello [Windows Store bevezető oktatóanyag](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Ha segítségre van szüksége egy új értesítési központ létrehozása.
3. Válassza ki **beállítások**.
4. Kattintson a **hozzáférési házirendek**. Mindkét megosztott és teljes körű hozzáférést kapcsolati karakterláncok jelenik meg.

![Azure portál – Notification hubs használatával](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> Beolvasni a hello kapcsolati karakterláncot hello **Get-AzureSbNamespace** parancsmag által biztosított [Azure PowerShell](/powershell/azureps-cmdlets-docs) vagy hello **azure sb névtér megjelenítése** parancsot Hello [Azure parancssori felület (CLI)](../cli-install-nodejs.md).
> 
> 

## <a name="general-architecture"></a>Általános architektúra
Hello **NotificationHubService** vezérlőnek hello küldéséhez a leküldéses értesítések toospecific eszközök és alkalmazások figyelőobjektum-példányok a következő:

* **Android** -hello használata **GcmService** objektum, amely érhető el **notificationHubService.gcm**
* **iOS** -hello használata **ApnsService** objektum, amely elérhető a **notificationHubService.apns**
* **Windows Phone** -hello használata **MpnsService** objektum, amely érhető el **notificationHubService.mpns**
* **Univerzális Windows Platform** -hello használata **WnsService** objektum, amely érhető el **notificationHubService.wns**

### <a name="how-to-send-push-notifications-tooandroid-applications"></a>Hogyan: leküldéses értesítések tooAndroid alkalmazások küldése
Hello **GcmService** objektum tartalmaz egy **küldése** módszerhez használt toosend leküldéses értesítések tooAndroid alkalmazások lehet. Hello **küldése** metódus hello a következő paraméterek fogadja el:

* **Címkék** -hello címke azonosítója. Nincs címke van megadva, ha hello értesítést küld tooall ügyfelek.
* **Hasznos** -hello üzenet JSON vagy raw hasznos adatcsomag található.
* **A visszahívási** -hello visszahívási függvény.

Hello az adattartalom formátuma további információkért lásd: hello **hasznos** hello szakasza [GCM-kiszolgáló végrehajtási](http://developer.android.com/google/gcm/server.html#payload) dokumentum.

hello következő használ a hello **GcmService** hello által elérhetővé tett példány **NotificationHubService** toosend egy leküldéses értesítési tooall regisztrált ügyfelek.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-tooios-applications"></a>Hogyan: leküldéses értesítések tooiOS alkalmazások küldése
Ugyanaz, mint a fenti, az Android-alkalmazások hello **ApnsService** objektum tartalmaz egy **küldése** módszerhez használt toosend leküldéses értesítések tooiOS alkalmazások lehet. Hello **küldése** metódus hello a következő paraméterek fogadja el:

* **Címkék** -hello címke azonosítója. Nincs címke van megadva, ha hello értesítést küld tooall ügyfelek.
* **Hasznos** - üzenet JSON hello vagy karakterlánc-hasznos.
* **A visszahívási** -hello visszahívási függvény.

További információ hello az adattartalom formátuma, lásd: hello **értesítési tartalom** hello szakasza [helyi és leküldéses értesítések programozásával foglalkozó útmutatójában](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentum.

hello következő használ a hello **ApnsService** hello által elérhetővé tett példány **NotificationHubService** toosend riasztást message tooall ügyfelek:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-toowindows-phone-applications"></a>Hogyan: küldése leküldéses értesítések tooWindows Phone rendszerre készült alkalmazások
Hello **MpnsService** objektum tartalmaz egy **küldése** módszerhez használt toosend leküldéses értesítések tooWindows Phone rendszerre készült alkalmazások is lehetnek. Hello **küldése** metódus hello a következő paraméterek fogadja el:

* **Címkék** -hello címke azonosítója. Nincs címke van megadva, ha hello értesítést küld tooall ügyfelek.
* **Hasznos** -üzenet XML-adattartalmat hello.
* **TargetName**  -  `toast` bejelentési értesítéseket. `token`a csempe értesítések.
* **NotificationClass** -hello hello értesítési prioritását. Lásd: hello **HTTP-fejléc elemek** hello szakasza [leküldéses értesítések kiszolgálóról](http://msdn.microsoft.com/library/hh221551.aspx) érvényes értékek a dokumentum.
* **Beállítások** – nem kötelező kérelem fejlécei.
* **A visszahívási** -hello visszahívási függvény.

Érvényes listája **TargetName**, **NotificationClass** és fejléc lehetőségekről, tekintse meg a hello [leküldéses értesítések kiszolgálóról](http://msdn.microsoft.com/library/hh221551.aspx) lap.

a következő példakód hello használ hello **MpnsService** hello által elérhetővé tett példány **NotificationHubService** toosend egy bejelentési leküldéses értesítést:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-toouniversal-windows-platform-uwp-applications"></a>Hogyan: küldése leküldéses értesítések tooUniversal Windows-Platform (UWP) alkalmazások
Hello **WnsService** objektum tartalmaz egy **küldése** módszerhez használt toosend leküldéses értesítések tooUniversal Windows Platform alkalmazások lehet.  Hello **küldése** metódus hello a következő paraméterek fogadja el:

* **Címkék** -hello címke azonosítója. Nincs címke van megadva, ha hello értesítést küld regisztrált tooall ügyfelek.
* **Hasznos** -hello XML üzenetadatokat.
* **Típus** -hello értesítés típusa.
* **Beállítások** – nem kötelező kérelem fejlécei.
* **A visszahívási** -hello visszahívási függvény.

Az érvényes típusok és kérelemfejléc listájáért lásd: [leküldéses értesítési szolgáltatás kérés- és válaszfejlécekről](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

hello következő használ a hello **WnsService** hello által elérhetővé tett példány **NotificationHubService** toosend egy bejelentési leküldéses értesítési tooa UWP-alkalmazást:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a>Következő lépések
hello minta kódtöredékek fenti meg tooeasily build szolgáltatás infrastruktúra toodeliver leküldéses értesítések tooa számos különféle eszközök engedélyezése. Most, hogy megismerte a Notification Hubs használata node.js hello alapjait, kövesse ezeket hivatkozások toolearn további kapcsolatban, hogyan bővítheti a további ezeket a képességeket.

* Lásd az MSDN-hivatkozást hello [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).
* A Microsoft hello [Azure SDK for csomópont] GitHub tárházából további mintákat és megvalósítás részletei.

[Azure SDK for csomópont]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain hello Default Management Credentials for hello Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application tooUse Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages tooa Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[Azure Classic Portal]: http://manage.windowsazure.com
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
[webhelyet a WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
