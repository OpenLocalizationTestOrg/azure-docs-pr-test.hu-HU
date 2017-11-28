---
title: "Az Azure Functions időzítő indítófeltételt |} Microsoft Docs"
description: "Időzítő eseményindítók használata az Azure Functions ismertetése."
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
ms.openlocfilehash: 6a97ab8508f889b77d064a5da70e3c726d62900c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-timer-trigger"></a><span data-ttu-id="4c104-104">Az Azure Functions időzítő indítófeltételt</span><span class="sxs-lookup"><span data-stu-id="4c104-104">Azure Functions timer trigger</span></span>

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="4c104-105">Ez a cikk azt ismerteti, konfigurálása és az Azure Functions kód időzítő eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="4c104-105">This article explains how to configure and code timer triggers in Azure Functions.</span></span> <span data-ttu-id="4c104-106">Az Azure Functions lehetővé teszi, hogy a kódra a függvény a meghatározott ütemezés szerint időzítő eseményindító-kötéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4c104-106">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> 

<span data-ttu-id="4c104-107">Az időzítő indítófeltételt többpéldányos kibővített támogatja. Egy adott időzítő egyetlen példányán fut minden példányára.</span><span class="sxs-lookup"><span data-stu-id="4c104-107">The timer trigger supports multi-instance scale-out. A single instance of a particular timer function is run across all instances.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a><span data-ttu-id="4c104-108">Időzítő eseményindító</span><span class="sxs-lookup"><span data-stu-id="4c104-108">Timer trigger</span></span>
<span data-ttu-id="4c104-109">Az időzítő indítófeltételt egy függvénynek, használja a következő JSON-objektum a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="4c104-109">The timer trigger to a function uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="4c104-110">Értékének `schedule` van egy [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression) , amely tartalmazza a hat mezők:</span><span class="sxs-lookup"><span data-stu-id="4c104-110">The value of `schedule` is a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that includes these six fields:</span></span> 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
><span data-ttu-id="4c104-111">A cron-kifejezés talál online számos hagyja ki ezt a `{second}` mező.</span><span class="sxs-lookup"><span data-stu-id="4c104-111">Many of the cron expressions you find online omit the `{second}` field.</span></span> <span data-ttu-id="4c104-112">Másolása egyik, ha úgy, hogy az extra kell `{second}` mező.</span><span class="sxs-lookup"><span data-stu-id="4c104-112">If you copy from one of them, you need to adjust for the extra `{second}` field.</span></span> <span data-ttu-id="4c104-113">Adott, tekintse meg a [példák ütemezése](#examples) alatt.</span><span class="sxs-lookup"><span data-stu-id="4c104-113">For specific examples, see [Schedule examples](#examples) below.</span></span>

<span data-ttu-id="4c104-114">Az alapértelmezett időzónát együtt a CRON-kifejezés az egyezményes világidő (UTC).</span><span class="sxs-lookup"><span data-stu-id="4c104-114">The default time zone used with the CRON expressions is Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="4c104-115">A CRON-kifejezés alapján másik időzónában van, hozzon létre egy új alkalmazásbeállítást nevű függvény alkalmazás `WEBSITE_TIME_ZONE`.</span><span class="sxs-lookup"><span data-stu-id="4c104-115">To have your CRON expression based on another time zone, create a new app setting for your function app named `WEBSITE_TIME_ZONE`.</span></span> <span data-ttu-id="4c104-116">Adja meg a értéket a kívánt időzóna neve látható módon a [Microsoft időzóna Index](https://msdn.microsoft.com/library/ms912391.aspx).</span><span class="sxs-lookup"><span data-stu-id="4c104-116">Set the value to the name of the desired time zone as shown in the [Microsoft Time Zone Index](https://msdn.microsoft.com/library/ms912391.aspx).</span></span> 

<span data-ttu-id="4c104-117">Például *keleti téli idő* az UTC-05:00.</span><span class="sxs-lookup"><span data-stu-id="4c104-117">For example, *Eastern Standard Time* is UTC-05:00.</span></span> <span data-ttu-id="4c104-118">Szeretné, hogy a időzítő tűz indítás: 10:00 de minden nap, használja a következő CRON-kifejezés, amely az UTC időzóna:</span><span class="sxs-lookup"><span data-stu-id="4c104-118">To have your timer trigger fire at 10:00 AM EST every day, use the following CRON expression that accounts for UTC time zone:</span></span>

```json
"schedule": "0 0 15 * * *",
``` 

<span data-ttu-id="4c104-119">Azt is megteheti, hozzáadhatja egy új alkalmazásbeállítást a függvény nevű alkalmazást `WEBSITE_TIME_ZONE` és állítsa be az értéket **keleti téli idő**.</span><span class="sxs-lookup"><span data-stu-id="4c104-119">Alternatively, you could add a new app setting for your function app named `WEBSITE_TIME_ZONE` and set the value to **Eastern Standard Time**.</span></span>  <span data-ttu-id="4c104-120">Ezután a következő CRON-kifejezés 10:00 de használható:</span><span class="sxs-lookup"><span data-stu-id="4c104-120">Then the following CRON expression could be used for 10:00 AM EST:</span></span> 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a><span data-ttu-id="4c104-121">Ütemezés példák</span><span class="sxs-lookup"><span data-stu-id="4c104-121">Schedule examples</span></span>
<span data-ttu-id="4c104-122">Az alábbiakban néhány CRON kifejezésekre is használhatja a mintákat a `schedule` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4c104-122">Here are some samples of CRON expressions you can use for the `schedule` property.</span></span> 

<span data-ttu-id="4c104-123">5 percenként egyszer indításához:</span><span class="sxs-lookup"><span data-stu-id="4c104-123">To trigger once every five minutes:</span></span>

```json
"schedule": "0 */5 * * * *"
```

<span data-ttu-id="4c104-124">Az Indítás egyszer minden órában tetején:</span><span class="sxs-lookup"><span data-stu-id="4c104-124">To trigger once at the top of every hour:</span></span>

```json
"schedule": "0 0 * * * *",
```

<span data-ttu-id="4c104-125">Két óránként indításához:</span><span class="sxs-lookup"><span data-stu-id="4c104-125">To trigger once every two hours:</span></span>

```json
"schedule": "0 0 */2 * * *",
```

<span data-ttu-id="4c104-126">Óránként egyszer a Reggel 9 délután 5 óra történő indításához:</span><span class="sxs-lookup"><span data-stu-id="4c104-126">To trigger once every hour from 9 AM to 5 PM:</span></span>

```json
"schedule": "0 0 9-17 * * *",
```

<span data-ttu-id="4c104-127">Indításához: 9:30 AM minden nap:</span><span class="sxs-lookup"><span data-stu-id="4c104-127">To trigger At 9:30 AM every day:</span></span>

```json
"schedule": "0 30 9 * * *",
```

<span data-ttu-id="4c104-128">Indításához: 9:30 AM minden hétköznap:</span><span class="sxs-lookup"><span data-stu-id="4c104-128">To trigger At 9:30 AM every weekday:</span></span>

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="4c104-129">Eseményindító kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="4c104-129">Trigger usage</span></span>
<span data-ttu-id="4c104-130">Egy időzítő funkció meghívásakor a [objektum](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) átad a funkciót a.</span><span class="sxs-lookup"><span data-stu-id="4c104-130">When a timer trigger function is invoked, the [timer object](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) is passed into the function.</span></span> <span data-ttu-id="4c104-131">A következő JSON-ja az objektum egy példa ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="4c104-131">The following JSON is an example representation of the timer object.</span></span> 

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

## <a name="trigger-sample"></a><span data-ttu-id="4c104-132">Eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="4c104-132">Trigger sample</span></span>
<span data-ttu-id="4c104-133">Tegyük fel, hogy rendelkezik a következő időzítő indítófeltételt a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="4c104-133">Suppose you have the following timer trigger in the `bindings` array of function.json:</span></span>

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

<span data-ttu-id="4c104-134">Tekintse meg a nyelvspecifikus mintát, hogy az olvasások az objektum, hogy késői fut-e.</span><span class="sxs-lookup"><span data-stu-id="4c104-134">See the language-specific sample that reads the timer object to see whether it's running late.</span></span>

* [<span data-ttu-id="4c104-135">C#</span><span class="sxs-lookup"><span data-stu-id="4c104-135">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="4c104-136">F#</span><span class="sxs-lookup"><span data-stu-id="4c104-136">F#</span></span>](#triggerfsharp)
* [<span data-ttu-id="4c104-137">Node.js</span><span class="sxs-lookup"><span data-stu-id="4c104-137">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a><span data-ttu-id="4c104-138">A C# eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="4c104-138">Trigger sample in C#</span></span> #
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

### <a name="trigger-sample-in-f"></a><span data-ttu-id="4c104-139">Az F # eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="4c104-139">Trigger sample in F#</span></span> #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a><span data-ttu-id="4c104-140">A node.js eseményindító minta</span><span class="sxs-lookup"><span data-stu-id="4c104-140">Trigger sample in Node.js</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="4c104-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c104-141">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

