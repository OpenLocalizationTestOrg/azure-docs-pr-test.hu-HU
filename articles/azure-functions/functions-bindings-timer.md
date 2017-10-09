---
title: "aaaAzure funkciók időzítő indítófeltételt |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse időzítő elindítja az Azure Functions."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 17fca22372dbc55d4684c8c099cc97923a7d3cf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-timer-trigger"></a>Az Azure Functions időzítő indítófeltételt

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan tooconfigure és kód időzítő elindítja az Azure Functions. Az Azure Functions lehetővé teszi, hogy a kódra a függvény a meghatározott ütemezés szerint időzítő eseményindító-kötéssel rendelkezik. 

hello időzítő indítófeltételt többpéldányos kibővített támogatja. Egy adott időzítő egyetlen példányán fut minden példányára.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a>Időzítő eseményindító
hello időzítő eseményindító tooa függvény használja a következő JSON-objektum a hello hello `bindings` function.json tömbje:

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

hello értékének `schedule` van egy [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression) , amely tartalmazza a hat mezők: 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
>Hello cron kifejezések talál online számos hagyja el a hello `{second}` mező. Másolása egyik, ha szüksége tooadjust hello extra `{second}` mező. Adott, tekintse meg a [példák ütemezése](#examples) alatt.

hello hello CRON kifejezésekkel használt alapértelmezett időzóna az egyezményes világidő (UTC). toohave CRON-kifejezés egy másik időzónán alapuló váltását, hozzon létre egy új alkalmazásbeállítást nevű függvény alkalmazás `WEBSITE_TIME_ZONE`. Set hello érték toohello neve hello időzóna szükséges, ahogy az hello [Microsoft időzóna Index](https://msdn.microsoft.com/library/ms912391.aspx). 

Például *keleti téli idő* az UTC-05:00. toohave a időzítő indul el, tűz: 10:00 de naponta, a következő CRON-kifejezés az UTC időzóna fiókok használatát hello:

```json
"schedule": "0 0 15 * * *",
``` 

Azt is megteheti, hozzáadhatja egy új alkalmazásbeállítást a függvény nevű alkalmazást `WEBSITE_TIME_ZONE` és hello érték túl**keleti téli idő**.  Majd 10:00 de hello CRON-kifejezés a következő használható: 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a>Ütemezés példák
Az alábbiakban néhány minták CRON kifejezésekre is használhatja a hello `schedule` tulajdonság. 

5 percenként egyszer tootrigger:

```json
"schedule": "0 */5 * * * *"
```

hello felső részén minden órában egyszer, tootrigger:

```json
"schedule": "0 0 * * * *",
```

tootrigger két óránként:

```json
"schedule": "0 0 */2 * * *",
```

a Reggel 9 too5 óránként egyszer tootrigger PM:

```json
"schedule": "0 0 9-17 * * *",
```

tootrigger: 9:30 AM minden nap:

```json
"schedule": "0 30 9 * * *",
```

9:30 AM minden hétköznap, tootrigger:

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a>Eseményindító kihasználtsága
Egy időzítő funkció meghívásakor hello [objektum](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) átad a hello függvény. hello következő JSON-ja hello objektum egy példa ábrázolását. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

<a name="sample"></a>

## <a name="trigger-sample"></a>Eseményindító minta
Tegyük fel, hogy rendelkezik a következő időzítő indítófeltételt a hello hello `bindings` function.json tömbje:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Lásd: olvasó a objektum toosee hello időzítő attól késői hello nyelvspecifikus minta.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>A C# eseményindító minta #
```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Az F # eseményindító minta #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>A node.js eseményindító minta
```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

