---
title: "Az Azure Functions Twilio-kötés |} Microsoft Docs"
description: "Az Azure Functions Twilio-kötések használatának megismerése."
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: a60263aa-3de9-4e1b-a2bb-0b52e70d559b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/20/2016
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 870e47ec7f8ce41ee4acadc7b8ed59298958acbe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="send-sms-messages-from-azure-functions-using-the-twilio-output-binding"></a><span data-ttu-id="8d216-104">SMS küldése az Azure Functions használatával a Twilio üzeneteit kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8d216-104">Send SMS messages from Azure Functions using the Twilio output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="8d216-105">Ez a cikk azt ismerteti, hogyan konfigurálhatja és használhatja a Twilio-kötések az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8d216-105">This article explains how to configure and use Twilio bindings with Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="8d216-106">Az Azure Functions támogatja Twilio kimeneti kötések néhány sornyi kódot SMS szöveges üzenetek küldéséhez a funkciók engedélyezéséhez és a [Twilio](https://www.twilio.com/) fiók.</span><span class="sxs-lookup"><span data-stu-id="8d216-106">Azure Functions supports Twilio output bindings to enable your functions to send SMS text messages with a few lines of code and a [Twilio](https://www.twilio.com/) account.</span></span> 

## <a name="functionjson-for-the-twilio-output-binding"></a><span data-ttu-id="8d216-107">az a Twilio Function.JSON kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8d216-107">function.json for the Twilio output binding</span></span>
<span data-ttu-id="8d216-108">A function.json fájl tartalmazza a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="8d216-108">The function.json file provides the following properties:</span></span>

* <span data-ttu-id="8d216-109">`name`: A Twilio SMS szöveges üzenetek függvény kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="8d216-109">`name` : Variable name used in function code for the Twilio SMS text message.</span></span>
* <span data-ttu-id="8d216-110">`type`: meg kell *"twilioSms"*.</span><span class="sxs-lookup"><span data-stu-id="8d216-110">`type` : must be set to *"twilioSms"*.</span></span>
* <span data-ttu-id="8d216-111">`accountSid`: Ez az érték, amely tárolja a Twilio-fiók Sid alkalmazásbeállítás neve értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="8d216-111">`accountSid` : This value must be set to the name of an App Setting that holds your Twilio Account Sid.</span></span>
* <span data-ttu-id="8d216-112">`authToken`: Ez az érték, amely tárolja a Twilio-hitelesítési jogkivonat alkalmazásbeállítás neve értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="8d216-112">`authToken` : This value must be set to the name of an App Setting that holds your Twilio authentication token.</span></span>
* <span data-ttu-id="8d216-113">`to`: Ez az érték a telefonszámot, amelyet az SMS szöveg küldött értéke.</span><span class="sxs-lookup"><span data-stu-id="8d216-113">`to` : This value is set to the phone number that the SMS text is sent to.</span></span>
* <span data-ttu-id="8d216-114">`from`: Ez értéke a telefonszámot, amelyet az SMS szöveg küldi.</span><span class="sxs-lookup"><span data-stu-id="8d216-114">`from` : This value is set to the phone number that the SMS text is sent from.</span></span>
* <span data-ttu-id="8d216-115">`direction`: meg kell *"out"*.</span><span class="sxs-lookup"><span data-stu-id="8d216-115">`direction` : must be set to *"out"*.</span></span>
* <span data-ttu-id="8d216-116">`body`: Ez az érték használatával lehet SMS üzenetet a merevlemez code, ha nincs szüksége a függvény a kódban dinamikusan beállításához.</span><span class="sxs-lookup"><span data-stu-id="8d216-116">`body` : This value can be used to hard code the SMS text message if you don't need to set it dynamically in the code for your function.</span></span> 

<span data-ttu-id="8d216-117">Példa function.json:</span><span class="sxs-lookup"><span data-stu-id="8d216-117">Example function.json:</span></span>

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="8d216-118">Példa C# várólista eseményindító Twilio kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8d216-118">Example C# queue trigger with Twilio output binding</span></span>
#### <a name="synchronous"></a><span data-ttu-id="8d216-119">Szinkron</span><span class="sxs-lookup"><span data-stu-id="8d216-119">Synchronous</span></span>
<span data-ttu-id="8d216-120">A szinkron példakódot egy Azure Storage várólista eseményindító egy kimeneti paramétert használ egy szöveges üzenetet küldeni a megrendelés ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="8d216-120">This synchronous example code for an Azure Storage queue trigger uses an out parameter to send a text message to a customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

#### <a name="asynchronous"></a><span data-ttu-id="8d216-121">Aszinkron</span><span class="sxs-lookup"><span data-stu-id="8d216-121">Asynchronous</span></span>
<span data-ttu-id="8d216-122">Az aszinkron példakódot egy Azure Storage várólista eseményindító szöveges üzenetet küld egy rendelést ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="8d216-122">This asynchronous example code for an Azure Storage queue trigger sends a text message to a customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="8d216-123">Példa Node.js várólista eseményindító Twilio kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8d216-123">Example Node.js queue trigger with Twilio output binding</span></span>
<span data-ttu-id="8d216-124">Ez a Node.js-példa szöveges üzenetet küld egy rendelést ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="8d216-124">This Node.js example sends a text message to a customer who placed an order.</span></span>

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="8d216-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d216-125">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

