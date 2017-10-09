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
# <a name="azure-functions-notification-hub-output-binding"></a><span data-ttu-id="c0f70-104">Az Azure Functions értesítési központ kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="c0f70-104">Azure Functions Notification Hub output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="c0f70-105">Ez a cikk azt ismerteti, hogyan tooconfigure és kód az Azure Functions Azure Notification Hub-kötéseket.</span><span class="sxs-lookup"><span data-stu-id="c0f70-105">This article explains how tooconfigure and code Azure Notification Hub bindings in Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="c0f70-106">A funkciók egy konfigurált Azure Notification Hub használata néhány sornyi kód leküldéses értesítéseket küldhet.</span><span class="sxs-lookup"><span data-stu-id="c0f70-106">Your functions can send push notifications using a configured Azure Notification Hub with a few lines of code.</span></span> <span data-ttu-id="c0f70-107">Azonban hello Azure Notification Hub kell konfigurálni hello toouse kívánt Platform értesítések szolgáltatások (PNS).</span><span class="sxs-lookup"><span data-stu-id="c0f70-107">However, hello Azure Notification Hub must be configured for hello Platform Notifications Services (PNS) you want toouse.</span></span> <span data-ttu-id="c0f70-108">Az Azure Notification Hub konfigurálása és egy ügyfél tooreceive értesítések regisztrálása alkalmazások fejlesztéséhez további információkért lásd: [Ismerkedés a Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kattintson a célplatform ügyfél hello: felső.</span><span class="sxs-lookup"><span data-stu-id="c0f70-108">For more information on configuring an Azure Notification Hub and developing a client applications that register tooreceive notifications, see [Getting started with Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) and click your target client platform at hello top.</span></span>

<span data-ttu-id="c0f70-109">hello értesítéseket küld a natív értesítések vagy sablon értesítések lehet.</span><span class="sxs-lookup"><span data-stu-id="c0f70-109">hello notifications you send can be native notifications or template notifications.</span></span> <span data-ttu-id="c0f70-110">Natív értesítések célplatform egy adott értesítés be hello `platform` hello tulajdonságának kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="c0f70-110">Native notifications target a specific notification platform as configured in hello `platform` property of hello output binding.</span></span> <span data-ttu-id="c0f70-111">Egy sablon értesítés lehet használt tootarget több platformon.</span><span class="sxs-lookup"><span data-stu-id="c0f70-111">A template notification can be used tootarget multiple platforms.</span></span>   

## <a name="notification-hub-output-binding-properties"></a><span data-ttu-id="c0f70-112">Értesítési központ kimeneti kötési tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="c0f70-112">Notification hub output binding properties</span></span>
<span data-ttu-id="c0f70-113">hello function.json fájl megad hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="c0f70-113">hello function.json file provides hello following properties:</span></span>

* <span data-ttu-id="c0f70-114">`name`: Értesítési központ üdvözlőüzenetére függvény a kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="c0f70-114">`name` : Variable name used in function code for hello notification hub message.</span></span>
* <span data-ttu-id="c0f70-115">`type`: be kell állítani túl*"notificationHub"*.</span><span class="sxs-lookup"><span data-stu-id="c0f70-115">`type` : must be set too*"notificationHub"*.</span></span>
* <span data-ttu-id="c0f70-116">`tagExpression`: Címke kifejezések lehetővé teszik, hogy értesítések kézbesíteni tooa készlete, amelyek megfelelnek hello címke kifejezésnek tooreceive értesítések regisztrált eszközök toospecify.</span><span class="sxs-lookup"><span data-stu-id="c0f70-116">`tagExpression` : Tag expressions allow you toospecify that notifications be delivered tooa set of devices who have registered tooreceive notifications that match hello tag expression.</span></span>  <span data-ttu-id="c0f70-117">További információkért lásd: [Útválasztás és címke kifejezések](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="c0f70-117">For more information, see [Routing and tag expressions](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span></span>
* <span data-ttu-id="c0f70-118">`hubName`: Hello notification hub erőforrás hello Azure-portálon a neve.</span><span class="sxs-lookup"><span data-stu-id="c0f70-118">`hubName` : Name of hello notification hub resource in hello Azure portal.</span></span>
* <span data-ttu-id="c0f70-119">`connection`: Ez a kapcsolati karakterláncnak kell lennie egy **Alkalmazásbeállítás** kapcsolati karakterlánc beállítása toohello *DefaultFullSharedAccessSignature* értéke az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="c0f70-119">`connection` : This connection string must be an **Application Setting** connection string set toohello *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
* <span data-ttu-id="c0f70-120">`direction`: be kell állítani túl*"out"*.</span><span class="sxs-lookup"><span data-stu-id="c0f70-120">`direction` : must be set too*"out"*.</span></span> 
* <span data-ttu-id="c0f70-121">`platform`: hello platform tulajdonság jelzi hello értesítési platform az értesítési célokat.</span><span class="sxs-lookup"><span data-stu-id="c0f70-121">`platform` : hello platform property indicates hello notification platform your notification targets.</span></span> <span data-ttu-id="c0f70-122">Hello a következő értékek egyike lehet:</span><span class="sxs-lookup"><span data-stu-id="c0f70-122">Must be one of hello following values:</span></span>
  * <span data-ttu-id="c0f70-123">Alapértelmezés szerint ha hello platform tulajdonság hiányzik a kötés, hello kimenet sablon értesítések lehet használt tootarget hello Azure Notification Hub konfigurált bármely platformra.</span><span class="sxs-lookup"><span data-stu-id="c0f70-123">By default, if hello platform property is omitted from hello output binding, template notifications can be used tootarget any platform configured on hello Azure Notification Hub.</span></span> <span data-ttu-id="c0f70-124">További információ a sablonok használatával általában toosend közötti platform értesítések az Azure Notification Hub, az: [sablonok](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="c0f70-124">For more information on using templates in general toosend cross platform notifications with an Azure Notification Hub, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>
  * <span data-ttu-id="c0f70-125">`apns`: Az Apple Push Notification szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c0f70-125">`apns` : Apple Push Notification Service.</span></span> <span data-ttu-id="c0f70-126">Hello értesítési központ konfigurálása APNS és hello értesítést kap egy ügyfél alkalmazásban további információkért lásd: [küldő leküldéses értesítések tooiOS az Azure Notification hubs használatával](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="c0f70-126">For more information on configuring hello notification hub for APNS and receiving hello notification in a client app, see [Sending push notifications tooiOS with Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span></span> 
  * <span data-ttu-id="c0f70-127">`adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span><span class="sxs-lookup"><span data-stu-id="c0f70-127">`adm` : [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span></span> <span data-ttu-id="c0f70-128">Hello értesítési központ konfigurálása az ADM és hello értesítést kap a Kindle-alkalmazást a további információkért lásd: [Ismerkedés a Notification Hubs szolgáltatással Kindle-alkalmazásokhoz](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="c0f70-128">For more information on configuring hello notification hub for ADM and receiving hello notification in a Kindle app, see [Getting Started with Notification Hubs for Kindle apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span></span> 
  * <span data-ttu-id="c0f70-129">`gcm`: [A Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span><span class="sxs-lookup"><span data-stu-id="c0f70-129">`gcm` : [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span></span> <span data-ttu-id="c0f70-130">Firebase Cloud Messaging, amely hello GCM új verziója, is támogatott.</span><span class="sxs-lookup"><span data-stu-id="c0f70-130">Firebase Cloud Messaging, which is hello new version of GCM, is also supported.</span></span> <span data-ttu-id="c0f70-131">A GCM/FCM hello értesítési központ konfigurálása és az Android-ügyfélalkalmazás hello értesítést további információkért lásd: [küldő leküldéses értesítések tooAndroid az Azure Notification hubs használatával](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="c0f70-131">For more information on configuring hello notification hub for GCM/FCM and receiving hello notification in an Android client app, see [Sending push notifications tooAndroid with Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span></span>
  * <span data-ttu-id="c0f70-132">`wns`: [Windows leküldéses értesítéseket kezelő szolgáltatása](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) célzó Windows platformra.</span><span class="sxs-lookup"><span data-stu-id="c0f70-132">`wns` : [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) targeting Windows platforms.</span></span> <span data-ttu-id="c0f70-133">Windows Phone 8.1 és újabb verziók WNS is támogatja.</span><span class="sxs-lookup"><span data-stu-id="c0f70-133">Windows Phone 8.1 and later is also supported by WNS.</span></span> <span data-ttu-id="c0f70-134">A WNS hello értesítési központ konfigurálása és hello értesítést kap egy univerzális Windows Platform (UWP) alkalmazásban további információkért lásd: [Ismerkedés a Notification Hubs Windows Universal Platform alkalmazásokkal való](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="c0f70-134">For more information on configuring hello notification hub for WNS and receiving hello notification in a Universal Windows Platform (UWP) app, see [Getting started with Notification Hubs for Windows Universal Platform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span></span>
  * <span data-ttu-id="c0f70-135">`mpns`: [A Microsoft leküldéses értesítéseket kezelő szolgáltatása](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span><span class="sxs-lookup"><span data-stu-id="c0f70-135">`mpns` : [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span></span> <span data-ttu-id="c0f70-136">Ez a platform támogatja a Windows Phone 8 és a korábbi Windows Phone-platformokat.</span><span class="sxs-lookup"><span data-stu-id="c0f70-136">This platform supports Windows Phone 8 and earlier Windows Phone platforms.</span></span> <span data-ttu-id="c0f70-137">Értesítési központ hello beállítása az mpns Szolgáltatáshoz, és hello értesítést kap a Windows Phone-alkalmazás további információkért lásd: [küldő leküldéses értesítések az Azure Notification Hubs – Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span><span class="sxs-lookup"><span data-stu-id="c0f70-137">For more information on configuring hello notification hub for MPNS and receiving hello notification in a Windows Phone app, see [Sending push notifications with Azure Notification Hubs on Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span></span>

<span data-ttu-id="c0f70-138">Példa function.json:</span><span class="sxs-lookup"><span data-stu-id="c0f70-138">Example function.json:</span></span>

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

## <a name="notification-hub-connection-string-setup"></a><span data-ttu-id="c0f70-139">Értesítési központ kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="c0f70-139">Notification hub connection string setup</span></span>
<span data-ttu-id="c0f70-140">egy értesítési központot toouse kimeneti kötése, konfigurálnia kell a kapcsolati karakterlánc hello hello központ.</span><span class="sxs-lookup"><span data-stu-id="c0f70-140">toouse a Notification hub output binding, you must configure hello connection string for hello hub.</span></span> <span data-ttu-id="c0f70-141">Ezt megteheti a hello *integráció* lapon válassza az értesítési központ, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="c0f70-141">This can be done on hello *Integrate* tab by selecting your notification hub or creating a new one.</span></span> 

<span data-ttu-id="c0f70-142">Hello kapcsolati karakterláncot adja hozzá manuálisan is hozzáadhat egy kapcsolati karakterláncot egy meglévő központi *DefaultFullSharedAccessSignature* tooyour értesítési központot.</span><span class="sxs-lookup"><span data-stu-id="c0f70-142">You can also manually add a connection string for an existing hub by adding a connection string for hello *DefaultFullSharedAccessSignature* tooyour notification hub.</span></span> <span data-ttu-id="c0f70-143">Ez a kapcsolati karakterlánc engedély toosend értesítési üzenetek a függvény hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="c0f70-143">This connection string provides your function access permission toosend notification messages.</span></span> <span data-ttu-id="c0f70-144">Hello *DefaultFullSharedAccessSignature* kapcsolódási karakterlánc értéke hello elérhető **kulcsok** gomb hello fő panelen a notification hub erőforrás hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c0f70-144">hello *DefaultFullSharedAccessSignature* connection string value can be accessed from hello **keys** button in hello main blade of your notification hub resource in hello Azure portal.</span></span> <span data-ttu-id="c0f70-145">toomanually hozzáadása egy kapcsolati karakterláncot a központ használata hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c0f70-145">toomanually add a connection string for your hub, use hello following steps:</span></span> 

1. <span data-ttu-id="c0f70-146">A hello **függvény app** panelen található hello Azure-portálon kattintson **függvény Alkalmazásbeállítások > Ugrás tooApp szolgáltatás beállításaira**.</span><span class="sxs-lookup"><span data-stu-id="c0f70-146">On hello **Function app** blade of hello Azure portal, click **Function App Settings > Go tooApp Service settings**.</span></span>
2. <span data-ttu-id="c0f70-147">A hello **beállítások** panelen kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="c0f70-147">In hello **Settings** blade, click **Application Settings**.</span></span>
3. <span data-ttu-id="c0f70-148">Görgessen lefelé toohello **Alkalmazásbeállítások** szakaszt, és adjon hozzá egy nevű bejegyzést a *DefaultFullSharedAccessSignature* értéke az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="c0f70-148">Scroll down toohello **App settings** section, and add a named entry for *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
4. <span data-ttu-id="c0f70-149">Az alkalmazás, karakterlánc neve beállítás hello kimeneti kötések hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c0f70-149">Reference your App setting string name in hello output bindings.</span></span> <span data-ttu-id="c0f70-150">Hasonló túl**MyHubConnectionString** fenti hello példában használt.</span><span class="sxs-lookup"><span data-stu-id="c0f70-150">Similar too**MyHubConnectionString** used in hello example above.</span></span>

## <a name="apns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="c0f70-151">APNS natív értesítések küldése a C# eseményindítók</span><span class="sxs-lookup"><span data-stu-id="c0f70-151">APNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="c0f70-152">Ez a példa bemutatja, hogyan toouse meghatározva a hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend natív APNS értesítést.</span><span class="sxs-lookup"><span data-stu-id="c0f70-152">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native APNS notification.</span></span> 

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

## <a name="gcm-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="c0f70-153">GCM natív értesítések küldése a C# eseményindítók</span><span class="sxs-lookup"><span data-stu-id="c0f70-153">GCM native notifications with C# queue triggers</span></span>
<span data-ttu-id="c0f70-154">Ez a példa bemutatja, hogyan toouse meghatározva a hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend natív GCM értesítést.</span><span class="sxs-lookup"><span data-stu-id="c0f70-154">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native GCM notification.</span></span> 

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

## <a name="wns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="c0f70-155">WNS natív értesítések küldése a C# eseményindítók</span><span class="sxs-lookup"><span data-stu-id="c0f70-155">WNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="c0f70-156">Ez a példa bemutatja, hogyan toouse meghatározva a hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend natív WNS poharad értesítést.</span><span class="sxs-lookup"><span data-stu-id="c0f70-156">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) toosend a native WNS toast notification.</span></span> 

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

## <a name="template-example-for-nodejs-timer-triggers"></a><span data-ttu-id="c0f70-157">Node.js időzítő eseményindítók sablon – példa</span><span class="sxs-lookup"><span data-stu-id="c0f70-157">Template example for Node.js timer triggers</span></span>
<span data-ttu-id="c0f70-158">Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) tartalmazó `location` és `message`.</span><span class="sxs-lookup"><span data-stu-id="c0f70-158">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

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

## <a name="template-example-for-f-timer-triggers"></a><span data-ttu-id="c0f70-159">Az F # időzítő eseményindítók sablon – példa</span><span class="sxs-lookup"><span data-stu-id="c0f70-159">Template example for F# timer triggers</span></span>
<span data-ttu-id="c0f70-160">Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) tartalmazó `location` és `message`.</span><span class="sxs-lookup"><span data-stu-id="c0f70-160">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a><span data-ttu-id="c0f70-161">Sablon példa egy kimeneti paraméter használatával</span><span class="sxs-lookup"><span data-stu-id="c0f70-161">Template example using an out parameter</span></span>
<span data-ttu-id="c0f70-162">Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , amely tartalmazza a `message` helyőrző hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="c0f70-162">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in hello template.</span></span>

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

## <a name="template-example-with-asynchronous-function"></a><span data-ttu-id="c0f70-163">Az aszinkron függvény sablon – példa</span><span class="sxs-lookup"><span data-stu-id="c0f70-163">Template example with asynchronous function</span></span>
<span data-ttu-id="c0f70-164">Aszinkron kódot használja, ha kimenő paraméterek nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="c0f70-164">If you are using asynchronous code, out parameters are not allowed.</span></span> <span data-ttu-id="c0f70-165">Ebben az esetben használjon `IAsyncCollector` tooreturn a sablon értesítést.</span><span class="sxs-lookup"><span data-stu-id="c0f70-165">In this case use `IAsyncCollector` tooreturn your template notification.</span></span> <span data-ttu-id="c0f70-166">hello következő kód egy aszinkron példában látható a fenti hello kódot.</span><span class="sxs-lookup"><span data-stu-id="c0f70-166">hello following code is an asynchronous example of hello code above.</span></span> 

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

## <a name="template-example-using-json"></a><span data-ttu-id="c0f70-167">Sablon példa JSON használatával</span><span class="sxs-lookup"><span data-stu-id="c0f70-167">Template example using JSON</span></span>
<span data-ttu-id="c0f70-168">Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , amely tartalmazza a `message` helyőrző hello sablon használatával érvényes JSON karakterláncnak.</span><span class="sxs-lookup"><span data-stu-id="c0f70-168">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in hello template using a valid JSON string.</span></span>

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a><span data-ttu-id="c0f70-169">Sablon példa dokumentumtár-típus a Notification Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="c0f70-169">Template example using Notification Hubs library types</span></span>
<span data-ttu-id="c0f70-170">Ez a példa bemutatja, hogyan toouse meghatározva a hello [Microsoft Azure Notification Hubs könyvtár](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="c0f70-170">This example shows how toouse types defined in hello [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="c0f70-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0f70-171">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

