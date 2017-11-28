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
# <a name="azure-functions-notification-hub-output-binding"></a><span data-ttu-id="538f0-104">Az Azure Functions értesítési központ kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="538f0-104">Azure Functions Notification Hub output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="538f0-105">Ez a cikk azt ismerteti, konfigurálása és az Azure Functions Azure Notification Hub kötések kódot.</span><span class="sxs-lookup"><span data-stu-id="538f0-105">This article explains how to configure and code Azure Notification Hub bindings in Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="538f0-106">A funkciók egy konfigurált Azure Notification Hub használata néhány sornyi kód leküldéses értesítéseket küldhet.</span><span class="sxs-lookup"><span data-stu-id="538f0-106">Your functions can send push notifications using a configured Azure Notification Hub with a few lines of code.</span></span> <span data-ttu-id="538f0-107">Azonban az Azure Notification Hub be kell állítani az a Platform értesítések szolgáltatások (PNS) szeretne használni.</span><span class="sxs-lookup"><span data-stu-id="538f0-107">However, the Azure Notification Hub must be configured for the Platform Notifications Services (PNS) you want to use.</span></span> <span data-ttu-id="538f0-108">Az Azure értesítési központ konfigurálása és egy, ha értesítést szeretne kapni regisztrálni alkalmazások fejlesztésére további információkért lásd: [Ismerkedés a Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) a célplatform ügyfél felső részén kattintson .</span><span class="sxs-lookup"><span data-stu-id="538f0-108">For more information on configuring an Azure Notification Hub and developing a client applications that register to receive notifications, see [Getting started with Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) and click your target client platform at the top.</span></span>

<span data-ttu-id="538f0-109">Az értesítéseket küld a natív értesítések vagy sablon értesítések lehet.</span><span class="sxs-lookup"><span data-stu-id="538f0-109">The notifications you send can be native notifications or template notifications.</span></span> <span data-ttu-id="538f0-110">Natív értesítések célplatform egy adott értesítés be a `platform` tulajdonság a kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="538f0-110">Native notifications target a specific notification platform as configured in the `platform` property of the output binding.</span></span> <span data-ttu-id="538f0-111">Sablon értesítést, amelyekre több platformon is használható.</span><span class="sxs-lookup"><span data-stu-id="538f0-111">A template notification can be used to target multiple platforms.</span></span>   

## <a name="notification-hub-output-binding-properties"></a><span data-ttu-id="538f0-112">Értesítési központ kimeneti kötési tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="538f0-112">Notification hub output binding properties</span></span>
<span data-ttu-id="538f0-113">A function.json fájl tartalmazza a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="538f0-113">The function.json file provides the following properties:</span></span>

* <span data-ttu-id="538f0-114">`name`: Az értesítési központ üzenet függvény kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="538f0-114">`name` : Variable name used in function code for the notification hub message.</span></span>
* <span data-ttu-id="538f0-115">`type`: meg kell *"notificationHub"*.</span><span class="sxs-lookup"><span data-stu-id="538f0-115">`type` : must be set to *"notificationHub"*.</span></span>
* <span data-ttu-id="538f0-116">`tagExpression`: Címke kifejezések adja meg, hogy az eszközök, amelyek megfelelnek a címke kifejezésnek értesítések fogadására regisztrált értesítések kézbesítendő teszik lehetővé.</span><span class="sxs-lookup"><span data-stu-id="538f0-116">`tagExpression` : Tag expressions allow you to specify that notifications be delivered to a set of devices who have registered to receive notifications that match the tag expression.</span></span>  <span data-ttu-id="538f0-117">További információkért lásd: [Útválasztás és címke kifejezések](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="538f0-117">For more information, see [Routing and tag expressions](../notification-hubs/notification-hubs-tags-segment-push-message.md).</span></span>
* <span data-ttu-id="538f0-118">`hubName`: Az Azure-portálon az értesítési központ erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="538f0-118">`hubName` : Name of the notification hub resource in the Azure portal.</span></span>
* <span data-ttu-id="538f0-119">`connection`: Ez a kapcsolati karakterláncnak kell lennie egy **Alkalmazásbeállítás** kapcsolati karakterlánc beállítása a *DefaultFullSharedAccessSignature* értéke az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="538f0-119">`connection` : This connection string must be an **Application Setting** connection string set to the *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
* <span data-ttu-id="538f0-120">`direction`: meg kell *"out"*.</span><span class="sxs-lookup"><span data-stu-id="538f0-120">`direction` : must be set to *"out"*.</span></span> 
* <span data-ttu-id="538f0-121">`platform`: A platform tulajdonság jelöli a értesítési platform az értesítési célokat.</span><span class="sxs-lookup"><span data-stu-id="538f0-121">`platform` : The platform property indicates the notification platform your notification targets.</span></span> <span data-ttu-id="538f0-122">A következő értékek egyikének kell lennie:</span><span class="sxs-lookup"><span data-stu-id="538f0-122">Must be one of the following values:</span></span>
  * <span data-ttu-id="538f0-123">Alapértelmezés szerint a platform tulajdonság nem szerepel a kimeneti kötés, ha sablon értesítések segítségével bármely célplatform az Azure értesítési központ konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="538f0-123">By default, if the platform property is omitted from the output binding, template notifications can be used to target any platform configured on the Azure Notification Hub.</span></span> <span data-ttu-id="538f0-124">A közötti az Azure Notification Hub platform értesítések küldése általában sablonokkal további információkért lásd: [sablonok](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="538f0-124">For more information on using templates in general to send cross platform notifications with an Azure Notification Hub, see [Templates](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).</span></span>
  * <span data-ttu-id="538f0-125">`apns`: Az Apple Push Notification szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="538f0-125">`apns` : Apple Push Notification Service.</span></span> <span data-ttu-id="538f0-126">Az értesítési központ konfigurálása az APN szolgáltatás és az értesítés fogadásának egy ügyfél alkalmazásban további információkért lásd: [küldő leküldéses értesítések küldéséhez iOS az Azure Notification hubs használatával](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="538f0-126">For more information on configuring the notification hub for APNS and receiving the notification in a client app, see [Sending push notifications to iOS with Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md)</span></span> 
  * <span data-ttu-id="538f0-127">`adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span><span class="sxs-lookup"><span data-stu-id="538f0-127">`adm` : [Amazon Device Messaging](https://developer.amazon.com/device-messaging).</span></span> <span data-ttu-id="538f0-128">Az értesítési központ konfigurálása az ADM és az értesítés fogadásának Kindle-alkalmazást a további információkért lásd: [Ismerkedés a Notification Hubs szolgáltatással Kindle-alkalmazásokhoz](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="538f0-128">For more information on configuring the notification hub for ADM and receiving the notification in a Kindle app, see [Getting Started with Notification Hubs for Kindle apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md)</span></span> 
  * <span data-ttu-id="538f0-129">`gcm`: [A Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span><span class="sxs-lookup"><span data-stu-id="538f0-129">`gcm` : [Google Cloud Messaging](https://developers.google.com/cloud-messaging/).</span></span> <span data-ttu-id="538f0-130">Firebase Cloud Messaging, amely GCM új verziója, is támogatott.</span><span class="sxs-lookup"><span data-stu-id="538f0-130">Firebase Cloud Messaging, which is the new version of GCM, is also supported.</span></span> <span data-ttu-id="538f0-131">Az értesítési központ konfigurálása GCM/FCM és az értesítés fogadásának egy Android-ügyfélalkalmazás további információkért lásd: [küldő leküldéses értesítések androidra az Azure Notification hubs használatával](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="538f0-131">For more information on configuring the notification hub for GCM/FCM and receiving the notification in an Android client app, see [Sending push notifications to Android with Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)</span></span>
  * <span data-ttu-id="538f0-132">`wns`: [Windows leküldéses értesítéseket kezelő szolgáltatása](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) célzó Windows platformra.</span><span class="sxs-lookup"><span data-stu-id="538f0-132">`wns` : [Windows Push Notification Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) targeting Windows platforms.</span></span> <span data-ttu-id="538f0-133">Windows Phone 8.1 és újabb verziók WNS is támogatja.</span><span class="sxs-lookup"><span data-stu-id="538f0-133">Windows Phone 8.1 and later is also supported by WNS.</span></span> <span data-ttu-id="538f0-134">Az értesítési központ konfigurálása a wns-ből, és az értesítés fogadásának egy univerzális Windows Platform (UWP) alkalmazásban további információkért lásd: [Ismerkedés a Notification Hubs Windows Universal Platform alkalmazásokkal való](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span><span class="sxs-lookup"><span data-stu-id="538f0-134">For more information on configuring the notification hub for WNS and receiving the notification in a Universal Windows Platform (UWP) app, see [Getting started with Notification Hubs for Windows Universal Platform Apps](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)</span></span>
  * <span data-ttu-id="538f0-135">`mpns`: [A Microsoft leküldéses értesítéseket kezelő szolgáltatása](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span><span class="sxs-lookup"><span data-stu-id="538f0-135">`mpns` : [Microsoft Push Notification Service](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx).</span></span> <span data-ttu-id="538f0-136">Ez a platform támogatja a Windows Phone 8 és a korábbi Windows Phone-platformokat.</span><span class="sxs-lookup"><span data-stu-id="538f0-136">This platform supports Windows Phone 8 and earlier Windows Phone platforms.</span></span> <span data-ttu-id="538f0-137">Az értesítési központ konfigurálása az mpns Szolgáltatáshoz, és a Windows Phone-alkalmazás az értesítés fogadásának további információkért lásd: [küldő leküldéses értesítések az Azure Notification Hubs – Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span><span class="sxs-lookup"><span data-stu-id="538f0-137">For more information on configuring the notification hub for MPNS and receiving the notification in a Windows Phone app, see [Sending push notifications with Azure Notification Hubs on Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)</span></span>

<span data-ttu-id="538f0-138">Példa function.json:</span><span class="sxs-lookup"><span data-stu-id="538f0-138">Example function.json:</span></span>

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

## <a name="notification-hub-connection-string-setup"></a><span data-ttu-id="538f0-139">Értesítési központ kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="538f0-139">Notification hub connection string setup</span></span>
<span data-ttu-id="538f0-140">A Notification hub kimeneti kötés használatához konfigurálnia kell a kapcsolati karakterláncot a központ.</span><span class="sxs-lookup"><span data-stu-id="538f0-140">To use a Notification hub output binding, you must configure the connection string for the hub.</span></span> <span data-ttu-id="538f0-141">Az ehhez a *integráció* lapon válassza az értesítési központ, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="538f0-141">This can be done on the *Integrate* tab by selecting your notification hub or creating a new one.</span></span> 

<span data-ttu-id="538f0-142">Adja hozzá a kapcsolati karakterláncot egy kapcsolati karakterláncot egy meglévő központ kézzel is hozzáadhatja a *DefaultFullSharedAccessSignature* az értesítési központba.</span><span class="sxs-lookup"><span data-stu-id="538f0-142">You can also manually add a connection string for an existing hub by adding a connection string for the *DefaultFullSharedAccessSignature* to your notification hub.</span></span> <span data-ttu-id="538f0-143">Ez a kapcsolati karakterlánc az értesítések küldéséhez függvény hozzáférési engedélyt biztosít.</span><span class="sxs-lookup"><span data-stu-id="538f0-143">This connection string provides your function access permission to send notification messages.</span></span> <span data-ttu-id="538f0-144">A *DefaultFullSharedAccessSignature* kapcsolódási karakterlánc értéke elérhető a **kulcsok** gombra a fő paneljén az értesítési központ erőforrás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="538f0-144">The *DefaultFullSharedAccessSignature* connection string value can be accessed from the **keys** button in the main blade of your notification hub resource in the Azure portal.</span></span> <span data-ttu-id="538f0-145">Manuálisan adja hozzá a központ kapcsolati karakterláncot, használja az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="538f0-145">To manually add a connection string for your hub, use the following steps:</span></span> 

1. <span data-ttu-id="538f0-146">Az a **függvény app** panel az Azure portál, kattintson a **függvény Alkalmazásbeállítások > az App Service-beállítások**.</span><span class="sxs-lookup"><span data-stu-id="538f0-146">On the **Function app** blade of the Azure portal, click **Function App Settings > Go to App Service settings**.</span></span>
2. <span data-ttu-id="538f0-147">Az a **beállítások** panelen kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="538f0-147">In the **Settings** blade, click **Application Settings**.</span></span>
3. <span data-ttu-id="538f0-148">Görgessen le a **Alkalmazásbeállítások** szakaszt, és adjon hozzá egy nevű bejegyzést a *DefaultFullSharedAccessSignature* értéke az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="538f0-148">Scroll down to the **App settings** section, and add a named entry for *DefaultFullSharedAccessSignature* value for your notification hub.</span></span>
4. <span data-ttu-id="538f0-149">Az alkalmazás beállítás karakterlánc nevét a kimeneti kötéseiben hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="538f0-149">Reference your App setting string name in the output bindings.</span></span> <span data-ttu-id="538f0-150">Hasonló **MyHubConnectionString** a fenti példában használt.</span><span class="sxs-lookup"><span data-stu-id="538f0-150">Similar to **MyHubConnectionString** used in the example above.</span></span>

## <a name="apns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="538f0-151">APNS natív értesítések küldése a C# eseményindítók</span><span class="sxs-lookup"><span data-stu-id="538f0-151">APNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="538f0-152">A példa bemutatja, hogyan használható a megadott típus beolvasása a [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) natív APNS értesítés küldése.</span><span class="sxs-lookup"><span data-stu-id="538f0-152">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native APNS notification.</span></span> 

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

## <a name="gcm-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="538f0-153">GCM natív értesítések küldése a C# eseményindítók</span><span class="sxs-lookup"><span data-stu-id="538f0-153">GCM native notifications with C# queue triggers</span></span>
<span data-ttu-id="538f0-154">A példa bemutatja, hogyan használható a megadott típus beolvasása a [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) natív GCM értesítés küldése.</span><span class="sxs-lookup"><span data-stu-id="538f0-154">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native GCM notification.</span></span> 

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

## <a name="wns-native-notifications-with-c-queue-triggers"></a><span data-ttu-id="538f0-155">WNS natív értesítések küldése a C# eseményindítók</span><span class="sxs-lookup"><span data-stu-id="538f0-155">WNS native notifications with C# queue triggers</span></span>
<span data-ttu-id="538f0-156">A példa bemutatja, hogyan használható a megadott típus beolvasása a [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) natív WNS bejelentési értesítés küldése.</span><span class="sxs-lookup"><span data-stu-id="538f0-156">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) to send a native WNS toast notification.</span></span> 

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

## <a name="template-example-for-nodejs-timer-triggers"></a><span data-ttu-id="538f0-157">Node.js időzítő eseményindítók sablon – példa</span><span class="sxs-lookup"><span data-stu-id="538f0-157">Template example for Node.js timer triggers</span></span>
<span data-ttu-id="538f0-158">Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) tartalmazó `location` és `message`.</span><span class="sxs-lookup"><span data-stu-id="538f0-158">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

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

## <a name="template-example-for-f-timer-triggers"></a><span data-ttu-id="538f0-159">Az F # időzítő eseményindítók sablon – példa</span><span class="sxs-lookup"><span data-stu-id="538f0-159">Template example for F# timer triggers</span></span>
<span data-ttu-id="538f0-160">Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) tartalmazó `location` és `message`.</span><span class="sxs-lookup"><span data-stu-id="538f0-160">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains `location` and `message`.</span></span>

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a><span data-ttu-id="538f0-161">Sablon példa egy kimeneti paraméter használatával</span><span class="sxs-lookup"><span data-stu-id="538f0-161">Template example using an out parameter</span></span>
<span data-ttu-id="538f0-162">Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , amely tartalmazza a `message` helyőrző a sablonban.</span><span class="sxs-lookup"><span data-stu-id="538f0-162">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in the template.</span></span>

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

## <a name="template-example-with-asynchronous-function"></a><span data-ttu-id="538f0-163">Az aszinkron függvény sablon – példa</span><span class="sxs-lookup"><span data-stu-id="538f0-163">Template example with asynchronous function</span></span>
<span data-ttu-id="538f0-164">Aszinkron kódot használja, ha kimenő paraméterek nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="538f0-164">If you are using asynchronous code, out parameters are not allowed.</span></span> <span data-ttu-id="538f0-165">Ebben az esetben használjon `IAsyncCollector` a sablon értesítési vissza.</span><span class="sxs-lookup"><span data-stu-id="538f0-165">In this case use `IAsyncCollector` to return your template notification.</span></span> <span data-ttu-id="538f0-166">A következő kódot a fenti kódot aszinkron példája.</span><span class="sxs-lookup"><span data-stu-id="538f0-166">The following code is an asynchronous example of the code above.</span></span> 

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

## <a name="template-example-using-json"></a><span data-ttu-id="538f0-167">Sablon példa JSON használatával</span><span class="sxs-lookup"><span data-stu-id="538f0-167">Template example using JSON</span></span>
<span data-ttu-id="538f0-168">Ebben a példában értesítést küld egy [sablon regisztrációs](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) , amely tartalmazza a `message` helyőrző a sablonban érvényes JSON karakterláncnak használatával.</span><span class="sxs-lookup"><span data-stu-id="538f0-168">This example sends a notification for a [template registration](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) that contains a `message` place holder in the template using a valid JSON string.</span></span>

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a><span data-ttu-id="538f0-169">Sablon példa dokumentumtár-típus a Notification Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="538f0-169">Template example using Notification Hubs library types</span></span>
<span data-ttu-id="538f0-170">A példa bemutatja, hogyan használható a megadott típus beolvasása a [Microsoft Azure Notification Hubs könyvtár](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="538f0-170">This example shows how to use types defined in the [Microsoft Azure Notification Hubs Library](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="538f0-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="538f0-171">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

