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
# <a name="send-sms-messages-from-azure-functions-using-hello-twilio-output-binding"></a>SMS küldése az Azure Functions használatával hello Twilio kimeneti kötése
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan tooconfigure és használja az Azure Functions Twilio-kötései. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Az Azure Functions támogatja a Twilio kimeneti kötések tooenable a funkciók toosend SMS szöveges üzenetek néhány sornyi kódot és egy [Twilio](https://www.twilio.com/) fiók. 

## <a name="functionjson-for-hello-twilio-output-binding"></a>a hello Function.JSON Twilio kimeneti kötése
hello function.json fájl megad hello következő tulajdonságai:

* `name`: Hello Twilio SMS szöveges üzenet függvény a kódban használt változó neve.
* `type`: be kell állítani túl*"twilioSms"*.
* `accountSid`: Ez az érték, amely tárolja a Twilio-fiók Sid alkalmazásbeállítás neve toohello kell beállítani.
* `authToken`: Ez az érték, amely tárolja a Twilio-hitelesítési jogkivonat alkalmazásbeállítás neve toohello kell beállítani.
* `to`: Az alapérték hello SMS szöveg küldött toohello telefonszámot.
* `from`: Az alapérték toohello telefonszámot, amelyet a küldi hello SMS szöveg.
* `direction`: be kell állítani túl*"out"*.
* `body`: Az érték lehet használt toohard kód hello SMS szöveges üzenet, ha nincs szüksége legyen dinamikusan hello code tooset a függvény. 

Példa function.json:

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


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Példa C# várólista eseményindító Twilio kimeneti kötése
#### <a name="synchronous"></a>Szinkron
A szinkron példakódot egy Azure Storage várólista eseményindító használ egy kimeneti paraméter toosend szöveges üzenet tooa ügyfélnek megrendelés.

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

#### <a name="asynchronous"></a>Aszinkron
Az aszinkron példakódot egy Azure Storage várólista eseményindító a szöveges üzenet tooa ügyfelünk megrendelés küld.

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

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Példa Node.js várólista eseményindító Twilio kimeneti kötése
Ez a Node.js-példa a szöveges üzenet tooa ügyfelünk megrendelés küld.

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

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

