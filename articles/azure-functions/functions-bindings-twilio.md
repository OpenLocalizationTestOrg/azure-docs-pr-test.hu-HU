---
title: "aaaAzure funkciók Twilio-kötés |} Microsoft Docs"
description: "Megértéséhez hogyan toouse Twilio-kötések az Azure Functions."
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
ms.openlocfilehash: 882853947850e7d6795ca5b2f3fb6b9a83ede182
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-sms-messages-from-azure-functions-using-hello-twilio-output-binding"></a><span data-ttu-id="c365a-104">SMS küldése az Azure Functions használatával hello Twilio kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="c365a-104">Send SMS messages from Azure Functions using hello Twilio output binding</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="c365a-105">Ez a cikk azt ismerteti, hogyan tooconfigure és használja az Azure Functions Twilio-kötései.</span><span class="sxs-lookup"><span data-stu-id="c365a-105">This article explains how tooconfigure and use Twilio bindings with Azure Functions.</span></span> 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<span data-ttu-id="c365a-106">Az Azure Functions támogatja a Twilio kimeneti kötések tooenable a funkciók toosend SMS szöveges üzenetek néhány sornyi kódot és egy [Twilio](https://www.twilio.com/) fiók.</span><span class="sxs-lookup"><span data-stu-id="c365a-106">Azure Functions supports Twilio output bindings tooenable your functions toosend SMS text messages with a few lines of code and a [Twilio](https://www.twilio.com/) account.</span></span> 

## <a name="functionjson-for-hello-twilio-output-binding"></a><span data-ttu-id="c365a-107">a hello Function.JSON Twilio kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="c365a-107">function.json for hello Twilio output binding</span></span>
<span data-ttu-id="c365a-108">hello function.json fájl megad hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="c365a-108">hello function.json file provides hello following properties:</span></span>

* <span data-ttu-id="c365a-109">`name`: Hello Twilio SMS szöveges üzenet függvény a kódban használt változó neve.</span><span class="sxs-lookup"><span data-stu-id="c365a-109">`name` : Variable name used in function code for hello Twilio SMS text message.</span></span>
* <span data-ttu-id="c365a-110">`type`: be kell állítani túl*"twilioSms"*.</span><span class="sxs-lookup"><span data-stu-id="c365a-110">`type` : must be set too*"twilioSms"*.</span></span>
* <span data-ttu-id="c365a-111">`accountSid`: Ez az érték, amely tárolja a Twilio-fiók Sid alkalmazásbeállítás neve toohello kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="c365a-111">`accountSid` : This value must be set toohello name of an App Setting that holds your Twilio Account Sid.</span></span>
* <span data-ttu-id="c365a-112">`authToken`: Ez az érték, amely tárolja a Twilio-hitelesítési jogkivonat alkalmazásbeállítás neve toohello kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="c365a-112">`authToken` : This value must be set toohello name of an App Setting that holds your Twilio authentication token.</span></span>
* <span data-ttu-id="c365a-113">`to`: Az alapérték hello SMS szöveg küldött toohello telefonszámot.</span><span class="sxs-lookup"><span data-stu-id="c365a-113">`to` : This value is set toohello phone number that hello SMS text is sent to.</span></span>
* <span data-ttu-id="c365a-114">`from`: Az alapérték toohello telefonszámot, amelyet a küldi hello SMS szöveg.</span><span class="sxs-lookup"><span data-stu-id="c365a-114">`from` : This value is set toohello phone number that hello SMS text is sent from.</span></span>
* <span data-ttu-id="c365a-115">`direction`: be kell állítani túl*"out"*.</span><span class="sxs-lookup"><span data-stu-id="c365a-115">`direction` : must be set too*"out"*.</span></span>
* <span data-ttu-id="c365a-116">`body`: Az érték lehet használt toohard kód hello SMS szöveges üzenet, ha nincs szüksége legyen dinamikusan hello code tooset a függvény.</span><span class="sxs-lookup"><span data-stu-id="c365a-116">`body` : This value can be used toohard code hello SMS text message if you don't need tooset it dynamically in hello code for your function.</span></span> 

<span data-ttu-id="c365a-117">Példa function.json:</span><span class="sxs-lookup"><span data-stu-id="c365a-117">Example function.json:</span></span>

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


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="c365a-118">Példa C# várólista eseményindító Twilio kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="c365a-118">Example C# queue trigger with Twilio output binding</span></span>
#### <a name="synchronous"></a><span data-ttu-id="c365a-119">Szinkron</span><span class="sxs-lookup"><span data-stu-id="c365a-119">Synchronous</span></span>
<span data-ttu-id="c365a-120">A szinkron példakódot egy Azure Storage várólista eseményindító használ egy kimeneti paraméter toosend szöveges üzenet tooa ügyfélnek megrendelés.</span><span class="sxs-lookup"><span data-stu-id="c365a-120">This synchronous example code for an Azure Storage queue trigger uses an out parameter toosend a text message tooa customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    message.Body = msg;
    message.too= order.mobileNumber;
}
```

#### <a name="asynchronous"></a><span data-ttu-id="c365a-121">Aszinkron</span><span class="sxs-lookup"><span data-stu-id="c365a-121">Asynchronous</span></span>
<span data-ttu-id="c365a-122">Az aszinkron példakódot egy Azure Storage várólista eseményindító a szöveges üzenet tooa ügyfelünk megrendelés küld.</span><span class="sxs-lookup"><span data-stu-id="c365a-122">This asynchronous example code for an Azure Storage queue trigger sends a text message tooa customer who placed an order.</span></span>

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.too= order.mobileNumber;

    await message.AddAsync(smsText);
}
```

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a><span data-ttu-id="c365a-123">Példa Node.js várólista eseményindító Twilio kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="c365a-123">Example Node.js queue trigger with Twilio output binding</span></span>
<span data-ttu-id="c365a-124">Ez a Node.js-példa a szöveges üzenet tooa ügyfelünk megrendelés küld.</span><span class="sxs-lookup"><span data-stu-id="c365a-124">This Node.js example sends a text message tooa customer who placed an order.</span></span>

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example hello queue item is a JSON string representing an order that contains hello name of a 
    // customer and a mobile number toosend text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want toouse a hard coded message and number in hello binding, you must at least 
    // initialize hello message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of hello body in hello output binding. In this example, we use 
    // hello order information toopersonalize a text message toohello mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        too: myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="c365a-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c365a-125">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

