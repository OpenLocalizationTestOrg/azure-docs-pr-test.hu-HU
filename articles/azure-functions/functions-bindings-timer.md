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
# <a name="azure-functions-timer-trigger"></a><span data-ttu-id="e8dda-104">Az Azure Functions időzítő indítófeltételt</span><span class="sxs-lookup"><span data-stu-id="e8dda-104">Azure Functions timer trigger</span></span>

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="e8dda-105">Ez a cikk azt ismerteti, hogyan tooconfigure és kód időzítő elindítja az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e8dda-105">This article explains how tooconfigure and code timer triggers in Azure Functions.</span></span> <span data-ttu-id="e8dda-106">Az Azure Functions lehetővé teszi, hogy a kódra a függvény a meghatározott ütemezés szerint időzítő eseményindító-kötéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e8dda-106">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> 

<span data-ttu-id="e8dda-107">hello időzítő indítófeltételt többpéldányos kibővített támogatja. Egy adott időzítő egyetlen példányán fut minden példányára.</span><span class="sxs-lookup"><span data-stu-id="e8dda-107">hello timer trigger supports multi-instance scale-out. A single instance of a particular timer function is run across all instances.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a><span data-ttu-id="e8dda-108">Időzítő eseményindító</span><span class="sxs-lookup"><span data-stu-id="e8dda-108">Timer trigger</span></span>
<span data-ttu-id="e8dda-109">hello időzítő eseményindító tooa függvény használja a következő JSON-objektum a hello hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="e8dda-109">hello timer trigger tooa function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="e8dda-110">hello értékének `schedule` van egy [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression) , amely tartalmazza a hat mezők:</span><span class="sxs-lookup"><span data-stu-id="e8dda-110">hello value of `schedule` is a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that includes these six fields:</span></span> 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
><span data-ttu-id="e8dda-111">Hello cron kifejezések talál online számos hagyja el a hello `{second}` mező.</span><span class="sxs-lookup"><span data-stu-id="e8dda-111">Many of hello cron expressions you find online omit hello `{second}` field.</span></span> <span data-ttu-id="e8dda-112">Másolása egyik, ha szüksége tooadjust hello extra `{second}` mező.</span><span class="sxs-lookup"><span data-stu-id="e8dda-112">If you copy from one of them, you need tooadjust for hello extra `{second}` field.</span></span> <span data-ttu-id="e8dda-113">Adott, tekintse meg a [példák ütemezése](#examples) alatt.</span><span class="sxs-lookup"><span data-stu-id="e8dda-113">For specific examples, see [Schedule examples](#examples) below.</span></span>

<span data-ttu-id="e8dda-114">hello hello CRON kifejezésekkel használt alapértelmezett időzóna az egyezményes világidő (UTC).</span><span class="sxs-lookup"><span data-stu-id="e8dda-114">hello default time zone used with hello CRON expressions is Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="e8dda-115">toohave CRON-kifejezés egy másik időzónán alapuló váltását, hozzon létre egy új alkalmazásbeállítást nevű függvény alkalmazás `WEBSITE_TIME_ZONE`.</span><span class="sxs-lookup"><span data-stu-id="e8dda-115">toohave your CRON expression based on another time zone, create a new app setting for your function app named `WEBSITE_TIME_ZONE`.</span></span> <span data-ttu-id="e8dda-116">Set hello érték toohello neve hello időzóna szükséges, ahogy az hello [Microsoft időzóna Index](https://msdn.microsoft.com/library/ms912391.aspx).</span><span class="sxs-lookup"><span data-stu-id="e8dda-116">Set hello value toohello name of hello desired time zone as shown in hello [Microsoft Time Zone Index](https://msdn.microsoft.com/library/ms912391.aspx).</span></span> 

<span data-ttu-id="e8dda-117">Például *keleti téli idő* az UTC-05:00.</span><span class="sxs-lookup"><span data-stu-id="e8dda-117">For example, *Eastern Standard Time* is UTC-05:00.</span></span> <span data-ttu-id="e8dda-118">toohave a időzítő indul el, tűz: 10:00 de naponta, a következő CRON-kifejezés az UTC időzóna fiókok használatát hello:</span><span class="sxs-lookup"><span data-stu-id="e8dda-118">toohave your timer trigger fire at 10:00 AM EST every day, use hello following CRON expression that accounts for UTC time zone:</span></span>

```json
"schedule": "0 0 15 * * *",
``` 

<span data-ttu-id="e8dda-119">Azt is megteheti, hozzáadhatja egy új alkalmazásbeállítást a függvény nevű alkalmazást `WEBSITE_TIME_ZONE` és hello érték túl**keleti téli idő**.</span><span class="sxs-lookup"><span data-stu-id="e8dda-119">Alternatively, you could add a new app setting for your function app named `WEBSITE_TIME_ZONE` and set hello value too**Eastern Standard Time**.</span></span>  <span data-ttu-id="e8dda-120">Majd 10:00 de hello CRON-kifejezés a következő használható:</span><span class="sxs-lookup"><span data-stu-id="e8dda-120">Then hello following CRON expression could be used for 10:00 AM EST:</span></span> 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a><span data-ttu-id="e8dda-121">Ütemezés példák</span><span class="sxs-lookup"><span data-stu-id="e8dda-121">Schedule examples</span></span>
<span data-ttu-id="e8dda-122">Az alábbiakban néhány minták CRON kifejezésekre is használhatja a hello `schedule` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e8dda-122">Here are some samples of CRON expressions you can use for hello `schedule` property.</span></span> 

<span data-ttu-id="e8dda-123">5 percenként egyszer tootrigger:</span><span class="sxs-lookup"><span data-stu-id="e8dda-123">tootrigger once every five minutes:</span></span>

```json
"schedule": "0 */5 * * * *"
```

<span data-ttu-id="e8dda-124">hello felső részén minden órában egyszer, tootrigger:</span><span class="sxs-lookup"><span data-stu-id="e8dda-124">tootrigger once at hello top of every hour:</span></span>

```json
"schedule": "0 0 * * * *",
```

<span data-ttu-id="e8dda-125">tootrigger két óránként:</span><span class="sxs-lookup"><span data-stu-id="e8dda-125">tootrigger once every two hours:</span></span>

```json
"schedule": "0 0 */2 * * *",
```

<span data-ttu-id="e8dda-126">a Reggel 9 too5 óránként egyszer tootrigger PM:</span><span class="sxs-lookup"><span data-stu-id="e8dda-126">tootrigger once every hour from 9 AM too5 PM:</span></span>

```json
"schedule": "0 0 9-17 * * *",
```

<span data-ttu-id="e8dda-127">tootrigger: 9:30 AM minden nap:</span><span class="sxs-lookup"><span data-stu-id="e8dda-127">tootrigger At 9:30 AM every day:</span></span>

```json
"schedule": "0 30 9 * * *",
```

<span data-ttu-id="e8dda-128">9:30 AM minden hétköznap, tootrigger:</span><span class="sxs-lookup"><span data-stu-id="e8dda-128">tootrigger At 9:30 AM every weekday:</span></span>

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="e8dda-129">Eseményindító kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="e8dda-129">Trigger usage</span></span>
<span data-ttu-id="e8dda-130">Egy időzítő funkció meghívásakor hello [objektum](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) átad a hello függvény.</span><span class="sxs-lookup"><span data-stu-id="e8dda-130">When a timer trigger function is invoked, hello [timer object](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) is passed into hello function.</span></span> <span data-ttu-id="e8dda-131">hello következő JSON-ja hello objektum egy példa ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="e8dda-131">hello following JSON is an example representation of hello timer object.</span></span> 

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

## <a name="trigger-sample"></a><span data-ttu-id="e8dda-132">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="e8dda-132">Trigger sample</span></span>
<span data-ttu-id="e8dda-133">Tegyük fel, hogy rendelkezik a következő időzítő indítófeltételt a hello hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="e8dda-133">Suppose you have hello following timer trigger in hello `bindings` array of function.json:</span></span>

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="e8dda-134">Lásd: olvasó a objektum toosee hello időzítő attól késői hello nyelvspecifikus minta.</span><span class="sxs-lookup"><span data-stu-id="e8dda-134">See hello language-specific sample that reads hello timer object toosee whether it's running late.</span></span>

* [<span data-ttu-id="e8dda-135">C#</span><span class="sxs-lookup"><span data-stu-id="e8dda-135">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="e8dda-136">F#</span><span class="sxs-lookup"><span data-stu-id="e8dda-136">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="e8dda-137">Node.js</span><span class="sxs-lookup"><span data-stu-id="e8dda-137">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="e8dda-138">A C# eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="e8dda-138">Trigger sample in C#</span></span> #
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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="e8dda-139">Az F # eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="e8dda-139">Trigger sample in F#</span></span> #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="e8dda-140">A node.js eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="e8dda-140">Trigger sample in Node.js</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="e8dda-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8dda-141">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

