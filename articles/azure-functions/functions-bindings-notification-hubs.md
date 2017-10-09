---
title: "aaaAzure funkciók értesítési központ kötés |} Microsoft Docs"
description: "Megértéséhez hogyan toouse Azure Notification Hub-kötés az Azure Functions."
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
ms.openlocfilehash: d192424a8ec701d02f8bcb4aa4c1d189b20537a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-notification-hub-output-binding"></a>Az Azure Functions értesítési központ kimeneti kötése
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan tooconfigure és kód az Azure Functions Azure Notification Hub-kötéseket. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

A funkciók egy konfigurált Azure Notification Hub használata néhány sornyi kód leküldéses értesítéseket küldhet. Azonban hello Azure Notification Hub kell konfigurálni hello toouse kívánt Platform értesítések szolgáltatások (PNS). Az Azure Notification Hub konfigurálása és egy ügyfél tooreceive értesítések regisztrálása alkalmazások fejlesztéséhez további információkért lásd: [Ismerkedés a Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kattintson a célplatform ügyfél hello: felső.

hello értesítéseket küld a natív értesítések vagy sablon értesítések lehet. Natív értesítések célplatform egy adott értesítés be hello `platform` hello tulajdonságának kimeneti kötése. Egy sablon értesítés lehet használt tootarget több platformon.   

## <a name="notification-hub-output-binding-properties"></a>Értesítési központ kimeneti kötési tulajdonságok
hello function.json fájl megad hello következő tulajdonságai:

* `name`: Értesítési központ üdvözlőüzenetére függvény a kódban használt változó neve.
* `type`: be kell állítani túl*"notificationHub"*.
* `tagExpression`: Címke kifejezések lehetővé teszik, hogy értesítések kézbesíteni tooa készlete, amelyek megfelelnek hello címke kifejezésnek tooreceive értesítések regisztrált eszközök toospecify.  További információkért lásd: [Útválasztás és címke kifejezések](../notification-hubs/notification-hubs-tags-segment-push-message.md).
* `hubName`: Hello notification hub erőforrás hello Azure-portálon a neve.
* `connection`: Ez a kapcsolati karakterláncnak kell lennie egy **Alkalmazásbeállítás** kapcsolati karakterlánc beállítása toohello *DefaultFullSharedAccessSignature* értéke az értesítési központban.
* `direction`: be kell állítani túl*"out"*. 
* `platform`: hello platform tulajdonság jelzi hello értesítési platform az értesítési célokat. Hello a következő értékek egyike lehet:
  * Alapértelmezés szerint ha hello platform tulajdonság hiányzik a kötés, hello kimenet sablon értesítések lehet használt tootarget hello Azure Notification Hub konfigurált bármely platformra. További információ a sablonok használatával általában toosend közötti platform értesítések az Azure Notification Hub, az: [sablonok](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
  * `apns`: Az Apple Push Notification szolgáltatás. Hello értesítési központ konfigurálása APNS és hello értesítést kap egy ügyfél alkalmazásban további információkért lásd: [küldő leküldéses értesítések tooiOS az Azure Notification hubs használatával](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
  * `adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging). Hello értesítési központ konfigurálása az ADM és hello értesítést kap a Kindle-alkalmazást a további információkért lásd: [Ismerkedés a Notification Hubs szolgáltatással Kindle-alkalmazásokhoz](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
  * `gcm`: [A Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, amely hello GCM új verziója, is támogatott. A GCM/FCM hello értesítési központ konfigurálása és az Android-ügyfélalkalmazás hello értesítést további információkért lásd: [küldő leküldéses értesítések tooAndroid az Azure Notification hubs használatával](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
  * `wns`: [Windows leküldéses értesítéseket kezelő szolgáltatása](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) célzó Windows platformra. Windows Phone 8.1 és újabb verziók WNS is támogatja. A WNS hello értesítési központ konfigurálása és hello értesítést kap egy univerzális Windows Platform (UWP) alkalmazásban további információkért lásd: [Ismerkedés a Notification Hubs Windows Universal Platform alkalmazásokkal való](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
  * `mpns`: [A Microsoft leküldéses értesítéseket kezelő szolgáltatása](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Ez a platform támogatja a Windows Phone 8 és a korábbi Windows Phone-platformokat. Értesítési központ hello beállítása az mpns Szolgáltatáshoz, és hello értesítést kap a Windows Phone-alkalmazás további információkért lásd: [küldő leküldéses értesítések az Azure Notification Hubs – Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)

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
egy értesítési központot toouse kimeneti kötése, konfigurálnia kell a kapcsolati karakterlánc hello hello központ. Ezt megteheti a hello *integráció* lapon válassza az értesítési központ, vagy hozzon létre egy újat. 

Hello kapcsolati karakterláncot adja hozzá manuálisan is hozzáadhat egy kapcsolati karakterláncot egy meglévő központi *DefaultFullSharedAccessSignature* tooyour értesítési központot. Ez a kapcsolati karakterlánc engedély toosend értesítési üzenetek a függvény hozzáférést biztosít. Hello *DefaultFullSharedAccessSignature* kapcsolódási karakterlánc értéke hello elérhető **kulcsok** gomb hello fő panelen a notification hub erőforrás hello Azure-portálon. toomanually hozzáadása egy kapcsolati karakterláncot a központ használata hello a következő lépéseket: 

1. A hello **függvény app** panelen található hello Azure-portálon kattintson **függvény Alkalmazásbeállítások > Ugrás tooApp szolgáltatás beállításaira**.
2. A hello **beállítások** panelen kattintson a **Alkalmazásbeállítások**.
3. Görgessen lefelé toohello **Alkalmazásbeállítások** szakaszt, és adjon hozzá egy nevű bejegyzést a *DefaultFullSharedAccessSignature* értéke az értesítési központban.
4. Az alkalmazás, karakterlánc neve beállítás hello kimeneti kötések hivatkozik. Hasonló túl**MyHubConnectionString** fenti hello példában használt.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>APNS natív értesítések küldése a C# eseményindítók
Ez a példa bemutatja, hogyan toouse meghatározva a hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend natív APNS értesítést. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants toobe added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM natív értesítések küldése a C# eseményindítók
Ez a példa bemutatja, hogyan toouse meghatározva a hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend natív GCM értesítést. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants toobe added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS natív értesítések küldése a C# eseményindítók
Ez a példa bemutatja, hogyan toouse meghatározva a hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend natív WNS poharad értesítést. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a new user toobe processed in hello form of a JSON string with 
    // a "name" value.
    //
    // hello XML format for a native WNS toast notification is ...
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
                                            "A new user wants toobe added (" + user.name + ")" + 
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
Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , amely tartalmazza a `message` helyőrző hello sablonban.

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
Aszinkron kódot használja, ha kimenő paraméterek nem engedélyezettek. Ebben az esetben használjon `IAsyncCollector` tooreturn a sablon értesítést. hello következő kód egy aszinkron példában látható a fenti hello kódot. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification tooNotification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants toobe added : " + message;
    return templateProperties;
}
```

## <a name="template-example-using-json"></a>Sablon példa JSON használatával
Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , amely tartalmazza a `message` helyőrző hello sablon használatával érvényes JSON karakterláncnak.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Sablon példa dokumentumtár-típus a Notification Hubs használatával
Ez a példa bemutatja, hogyan toouse meghatározva a hello [Microsoft Azure Notification Hubs könyvtár](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 

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

