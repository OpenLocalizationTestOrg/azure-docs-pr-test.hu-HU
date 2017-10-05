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
# <a name="send-sms-messages-from-azure-functions-using-the-twilio-output-binding"></a>SMS küldése az Azure Functions használatával a Twilio üzeneteit kimeneti kötése
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan konfigurálhatja és használhatja a Twilio-kötések az Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Az Azure Functions támogatja Twilio kimeneti kötések néhány sornyi kódot SMS szöveges üzenetek küldéséhez a funkciók engedélyezéséhez és a [Twilio](https://www.twilio.com/) fiók. 

## <a name="functionjson-for-the-twilio-output-binding"></a>az a Twilio Function.JSON kimeneti kötése
A function.json fájl tartalmazza a következő tulajdonságokkal:

* `name`: A Twilio SMS szöveges üzenetek függvény kódban használt változó neve.
* `type`: meg kell *"twilioSms"*.
* `accountSid`: Ez az érték, amely tárolja a Twilio-fiók Sid alkalmazásbeállítás neve értékre kell állítani.
* `authToken`: Ez az érték, amely tárolja a Twilio-hitelesítési jogkivonat alkalmazásbeállítás neve értékre kell állítani.
* `to`: Ez az érték a telefonszámot, amelyet az SMS szöveg küldött értéke.
* `from`: Ez értéke a telefonszámot, amelyet az SMS szöveg küldi.
* `direction`: meg kell *"out"*.
* `body`: Ez az érték használatával lehet SMS üzenetet a merevlemez code, ha nincs szüksége a függvény a kódban dinamikusan beállításához. 

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
A szinkron példakódot egy Azure Storage várólista eseményindító egy kimeneti paramétert használ egy szöveges üzenetet küldeni a megrendelés ügyfélnek.

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

#### <a name="asynchronous"></a>Aszinkron
Az aszinkron példakódot egy Azure Storage várólista eseményindító szöveges üzenetet küld egy rendelést ügyfélnek.

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

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Példa Node.js várólista eseményindító Twilio kimeneti kötése
Ez a Node.js-példa szöveges üzenetet küld egy rendelést ügyfélnek.

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

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

