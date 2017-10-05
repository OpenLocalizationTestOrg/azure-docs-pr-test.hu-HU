---
title: "Az Azure Functions értesítési központ kötés |} Microsoft Docs"
description: "Azure Notification Hub-kötés az Azure Functions használatának megismerése."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: 0ff0c949-20bf-430b-8dd5-d72b7b6ee6f7
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2016
ms.author: glenga
ms.openlocfilehash: fa3d37b963c1bb6b58127b9180cd657d7b1dabcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Az Azure Functions értesítési központ kimeneti kötése
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, konfigurálása és az Azure Functions Azure Notification Hub kötések kódot. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

A funkciók egy konfigurált Azure Notification Hub használata néhány sornyi kód leküldéses értesítéseket küldhet. Azonban az Azure Notification Hub be kell állítani az a Platform értesítések szolgáltatások (PNS) szeretne használni. Az Azure értesítési központ konfigurálása és egy, ha értesítést szeretne kapni regisztrálni alkalmazások fejlesztésére további információkért lásd: [Ismerkedés a Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) a célplatform ügyfél felső részén kattintson .

Az értesítéseket küld a natív értesítések vagy sablon értesítések lehet. Natív értesítések célplatform egy adott értesítés be a `platform` tulajdonság a kimeneti kötése. Sablon értesítést, amelyekre több platformon is használható.   

## <a name="notification-hub-output-binding-properties"></a>Értesítési központ kimeneti kötési tulajdonságok
A function.json fájl tartalmazza a következő tulajdonságokkal:

* `name`: Az értesítési központ üzenet függvény kódban használt változó neve.
* `type`: meg kell *"notificationHub"*.
* `tagExpression`: Címke kifejezések adja meg, hogy az eszközök, amelyek megfelelnek a címke kifejezésnek értesítések fogadására regisztrált értesítések kézbesítendő teszik lehetővé.  További információkért lásd: [Útválasztás és címke kifejezések](../notification-hubs/notification-hubs-tags-segment-push-message.md).
* `hubName`: Az Azure-portálon az értesítési központ erőforrás neve.
* `connection`: Ez a kapcsolati karakterláncnak kell lennie egy **Alkalmazásbeállítás** kapcsolati karakterlánc beállítása a *DefaultFullSharedAccessSignature* értéke az értesítési központban.
* `direction`: meg kell *"out"*. 
* `platform`: A platform tulajdonság jelöli a értesítési platform az értesítési célokat. A következő értékek egyikének kell lennie:
  * Alapértelmezés szerint a platform tulajdonság nem szerepel a kimeneti kötés, ha sablon értesítések segítségével bármely célplatform az Azure értesítési központ konfigurálva. A közötti az Azure Notification Hub platform értesítések küldése általában sablonokkal további információkért lásd: [sablonok](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
  * `apns`: Az Apple Push Notification szolgáltatás. Az értesítési központ konfigurálása az APN szolgáltatás és az értesítés fogadásának egy ügyfél alkalmazásban további információkért lásd: [küldő leküldéses értesítések küldéséhez iOS az Azure Notification hubs használatával](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
  * `adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging). Az értesítési központ konfigurálása az ADM és az értesítés fogadásának Kindle-alkalmazást a további információkért lásd: [Ismerkedés a Notification Hubs szolgáltatással Kindle-alkalmazásokhoz](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
  * `gcm`: [A Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, amely GCM új verziója, is támogatott. Az értesítési központ konfigurálása GCM/FCM és az értesítés fogadásának egy Android-ügyfélalkalmazás további információkért lásd: [küldő leküldéses értesítések androidra az Azure Notification hubs használatával](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
  * `wns`: [Windows leküldéses értesítéseket kezelő szolgáltatása](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) célzó Windows platformra. Windows Phone 8.1 és újabb verziók WNS is támogatja. Az értesítési központ konfigurálása a wns-ből, és az értesítés fogadásának egy univerzális Windows Platform (UWP) alkalmazásban további információkért lásd: [Ismerkedés a Notification Hubs Windows Universal Platform alkalmazásokkal való](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
  * `mpns`: [A Microsoft leküldéses értesítéseket kezelő szolgáltatása](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Ez a platform támogatja a Windows Phone 8 és a korábbi Windows Phone-platformokat. Az értesítési központ konfigurálása az mpns Szolgáltatáshoz, és a Windows Phone-alkalmazás az értesítés fogadásának további információkért lásd: [küldő leküldéses értesítések az Azure Notification Hubs – Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)

Példa function.json:

```json
{
  "bindings": [
    {
      "name": "notification",
      "type": "notificationHub",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "gcm",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="notification-hub-connection-string-setup"></a>Értesítési központ kapcsolati karakterlánc beállítása
A Notification hub kimeneti kötés használatához konfigurálnia kell a kapcsolati karakterláncot a központ. Az ehhez a *integráció* lapon válassza az értesítési központ, vagy hozzon létre egy újat. 

Adja hozzá a kapcsolati karakterláncot egy kapcsolati karakterláncot egy meglévő központ kézzel is hozzáadhatja a *DefaultFullSharedAccessSignature* az értesítési központba. Ez a kapcsolati karakterlánc az értesítések küldéséhez függvény hozzáférési engedélyt biztosít. A *DefaultFullSharedAccessSignature* kapcsolódási karakterlánc értéke elérhető a **kulcsok** gombra a fő paneljén az értesítési központ erőforrás az Azure portálon. Manuálisan adja hozzá a központ kapcsolati karakterláncot, használja az alábbi lépéseket: 

1. Az a **függvény app** panel az Azure portál, kattintson a **függvény Alkalmazásbeállítások > az App Service-beállítások**.
2. Az a **beállítások** panelen kattintson a **Alkalmazásbeállítások**.
3. Görgessen le a **Alkalmazásbeállítások** szakaszt, és adjon hozzá egy nevű bejegyzést a *DefaultFullSharedAccessSignature* értéke az értesítési központban.
4. Az alkalmazás beállítás karakterlánc nevét a kimeneti kötéseiben hivatkozik. Hasonló **MyHubConnectionString** a fenti példában használt.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>APNS natív értesítések küldése a C# eseményindítók
A példa bemutatja, hogyan használható a megadott típus beolvasása a [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) natív APNS értesítés küldése. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM natív értesítések küldése a C# eseményindítók
A példa bemutatja, hogyan használható a megadott típus beolvasása a [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) natív GCM értesítés küldése. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS natív értesítések küldése a C# eseményindítók
A példa bemutatja, hogyan használható a megadott típus beolvasása a [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) natív WNS bejelentési értesítés küldése. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The XML format for a native WNS toast notification is ...
    // <?xml version="1.0" encoding="utf-8"?>
    // <toast>
    //      <visual>
    //     <binding template="ToastText01">
    //       <text id="1">notification message</text>
    //     </binding>
    //   </visual>
    // </toast>

    log.Info($"Sending WNS toast notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                    "<toast><visual><binding template=\"ToastText01\">" +
                                        "<text id=\"1\">" + 
                                            "A new user wants to be added (" + user.name + ")" + 
                                        "</text>" +
                                    "</binding></visual></toast>";

    log.Info($"{wnsNotificationPayload}");
    await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));        
}
```

## <a name="template-example-for-nodejs-timer-triggers"></a>Node.js időzítő eseményindítók sablon – példa
Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) tartalmazó `location` és `message`.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);  
    context.bindings.notification = {
        location: "Redmond",
        message: "Hello from Node!"
    };
    context.done();
};
```

## <a name="template-example-for-f-timer-triggers"></a>Az F # időzítő eseményindítók sablon – példa
Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) tartalmazó `location` és `message`.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a>Sablon példa egy kimeneti paraméter használatával
Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , amely tartalmazza a `message` helyőrző a sablonban.

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = GetTemplateProperties(myQueueItem);
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return templateProperties;
}
```

## <a name="template-example-with-asynchronous-function"></a>Az aszinkron függvény sablon – példa
Aszinkron kódot használja, ha kimenő paraméterek nem engedélyezettek. Ebben az esetben használjon `IAsyncCollector` a sablon értesítési vissza. A következő kódot a fenti kódot aszinkron példája. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification to Notification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants to be added : " + message;
    return templateProperties;
}
```

## <a name="template-example-using-json"></a>Sablon példa JSON használatával
Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , amely tartalmazza a `message` helyőrző a sablonban érvényes JSON karakterláncnak használatával.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Sablon példa dokumentumtár-típus a Notification Hubs használatával
A példa bemutatja, hogyan használható a megadott típus beolvasása a [Microsoft Azure Notification Hubs könyvtár](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

```cs
#r "Microsoft.Azure.NotificationHubs"

using System;
using System.Threading.Tasks;
using Microsoft.Azure.NotificationHubs;

public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
{
   log.Info($"C# Queue trigger function processed: {myQueueItem}");
   notification = GetTemplateNotification(myQueueItem);
}

private static TemplateNotification GetTemplateNotification(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return new TemplateNotification(templateProperties);
}
```

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

