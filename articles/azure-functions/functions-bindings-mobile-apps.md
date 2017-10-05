---
title: "Az Azure Mobile Apps funkciók kötések |} Microsoft Docs"
description: "Azure Mobile Apps kötések az Azure Functions használatának megismerése."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: faad1263-0fa5-41a9-964f-aecbc0be706a
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/31/2016
ms.author: glenga
ms.openlocfilehash: c5e1c02984f9773b263c0bee7685c7d5ff62e658
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a><span data-ttu-id="9b914-104">Az Azure Mobile Apps funkciók kötések</span><span class="sxs-lookup"><span data-stu-id="9b914-104">Azure Functions Mobile Apps bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="9b914-105">Ez a cikk ismerteti, hogyan lehet konfigurálni és kód [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) Azure Functions kötések.</span><span class="sxs-lookup"><span data-stu-id="9b914-105">This article explains how to configure and code [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindings in Azure Functions.</span></span> <span data-ttu-id="9b914-106">Azure Functions támogatja bemeneti és kimeneti Mobile Apps kötéseit.</span><span class="sxs-lookup"><span data-stu-id="9b914-106">Azure Functions supports input and output bindings for Mobile Apps.</span></span>

<span data-ttu-id="9b914-107">A Mobile Apps bemeneti és kimeneti kötések lehetővé teszik, hogy [az olvasási és írási adattáblák](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) a mobilalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9b914-107">The Mobile Apps input and output bindings let you [read from and write to data tables](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) in your mobile app.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a><span data-ttu-id="9b914-108">Mobile Apps bemeneti kötése</span><span class="sxs-lookup"><span data-stu-id="9b914-108">Mobile Apps input binding</span></span>
<span data-ttu-id="9b914-109">A Mobile Apps bemeneti kötése tölt be egy olyan rekordot egy mobil table végpont a, és továbbadja azt a függvényt.</span><span class="sxs-lookup"><span data-stu-id="9b914-109">The Mobile Apps input binding loads a record from a mobile table endpoint and passes it into your function.</span></span> <span data-ttu-id="9b914-110">A C# és F # függvényekben a rekord módosításai rendszer automatikusan küldi vissza a tábla amikor sikeresen kilép, a függvény.</span><span class="sxs-lookup"><span data-stu-id="9b914-110">In a C# and F# functions, any changes made to the record are automatically sent back to the table when the function exits successfully.</span></span>

<span data-ttu-id="9b914-111">A Mobile Apps bemenete egy olyan függvényt használja a következő JSON-objektum a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="9b914-111">The Mobile Apps input to a function uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of the record to retrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

<span data-ttu-id="9b914-112">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="9b914-112">Note the following:</span></span>

* <span data-ttu-id="9b914-113">`id`lehet, hogy statikus, vagy az eseményindító, amely meghívja a függvényt alapulhat.</span><span class="sxs-lookup"><span data-stu-id="9b914-113">`id` can be static, or it can be based on the trigger that invokes the function.</span></span> <span data-ttu-id="9b914-114">Például, ha egy [várólista eseményindító]() a függvénynél, majd `"id": "{queueTrigger}"` az üzenetsorban lévő üzenetet karakterlánc értékét használja a rekord azonosító beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9b914-114">For example, if you use a [queue trigger]() for your function, then `"id": "{queueTrigger}"` uses the string value of the queue message as the record ID to retrieve.</span></span>
* <span data-ttu-id="9b914-115">`connection`a függvény alkalmazásban, viszont a mobilalkalmazás URL-CÍMÉT tartalmazó Alkalmazásbeállítás nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="9b914-115">`connection` should contain the name of an app setting in your function app, which in turn contains the URL of your mobile app.</span></span> <span data-ttu-id="9b914-116">A funkció URL-címet a mobilalkalmazás elleni szükséges REST-műveletek összeállításához.</span><span class="sxs-lookup"><span data-stu-id="9b914-116">The function uses this URL to construct the required REST operations against your mobile app.</span></span> <span data-ttu-id="9b914-117">Akkor [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() a mobilalkalmazás URL-címet tartalmaz, amely (mely tűnik `http://<appname>.azurewebsites.net`), majd adja meg az Alkalmazásbeállítás a nevét a `connection` tulajdonságot a bemeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="9b914-117">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify the name of the app setting in the `connection` property in your input binding.</span></span> 
* <span data-ttu-id="9b914-118">Meg kell adnia `apiKey` Ha Ön [valósít meg API-kulcs a Node.js mobile Apps-háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), vagy [valósít meg API-kulcs a .NET-mobil háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="9b914-118">You need to specify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="9b914-119">Ehhez meg [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() , amely tartalmazza az API-kulcsot, majd adja hozzá a `apiKey` tulajdonságot a bemeneti kötése az Alkalmazásbeállítás nevét.</span><span class="sxs-lookup"><span data-stu-id="9b914-119">To do this, you [create an app setting in your function app]() that contains the API key, then add the `apiKey` property in your input binding with the name of the app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="9b914-120">Az API-kulcs nem kell osztani a mobilalkalmazás-ügyfelekkel.</span><span class="sxs-lookup"><span data-stu-id="9b914-120">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="9b914-121">Ez csak kell elosztani biztonságosan Szolgáltatásoldali ügyfelek, például az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9b914-121">It should only be distributed securely to service-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="9b914-122">Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal.</span><span class="sxs-lookup"><span data-stu-id="9b914-122">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="9b914-123">Ez megvédi a bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="9b914-123">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="9b914-124">Bemeneti kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="9b914-124">Input usage</span></span>
<span data-ttu-id="9b914-125">Ez a szakasz bemutatja, hogyan használható a Mobile Apps bemeneti kötése a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="9b914-125">This section shows you how to use your Mobile Apps input binding in your function code.</span></span> 

<span data-ttu-id="9b914-126">A megadott tábla és a rekord Azonosítójú rekord található, amikor átadva azokat a megnevezett [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) paraméter (vagy a node.js, akkor átad a a `context.bindings.<name>` objektum).</span><span class="sxs-lookup"><span data-stu-id="9b914-126">When the record with the specified table and record ID is found, it is passed into the named [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (or, in Node.js, it is passed into the `context.bindings.<name>` object).</span></span> <span data-ttu-id="9b914-127">Ha a rekord nem található, a paraméter nem `null`.</span><span class="sxs-lookup"><span data-stu-id="9b914-127">When the record is not found, the parameter is `null`.</span></span> 

<span data-ttu-id="9b914-128">A C# és F # funkciók, a változtatások a bemeneti rekord (bemeneti paraméter) automatikusan küld vissza a Mobile Apps tábla amikor sikeresen kilép, a függvény.</span><span class="sxs-lookup"><span data-stu-id="9b914-128">In C# and F# functions, any changes you make to the input record (input parameter) is automatically sent back to the Mobile Apps table when the function exits successfully.</span></span> <span data-ttu-id="9b914-129">A Node.js funkciók használata `context.bindings.<name>` a bemeneti rekord eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9b914-129">In Node.js functions, use `context.bindings.<name>` to access the input record.</span></span> <span data-ttu-id="9b914-130">Node.js rekord nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="9b914-130">You cannot modify a record in Node.js.</span></span>

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="9b914-131">A minta bemeneti</span><span class="sxs-lookup"><span data-stu-id="9b914-131">Input sample</span></span>
<span data-ttu-id="9b914-132">Tegyük fel, amely lekéri a Mobile Apps tábla egyik rekordja, azonosító: a várólista eseményindító üzenet a következő function.json:</span><span class="sxs-lookup"><span data-stu-id="9b914-132">Suppose you have the following function.json, that retrieves a Mobile App table record with the id of the queue trigger message:</span></span>

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
        "name": "record",
        "type": "mobileTable",
        "tableName": "MyTable",
        "id" : "{queueTrigger}",
        "connection": "My_MobileApp_Url",
        "apiKey": "My_MobileApp_Key",
        "direction": "in"
    }
],
"disabled": false
}
```

<span data-ttu-id="9b914-133">Tekintse meg a nyelvspecifikus mintát, amely a bemeneti rekord a kötés használja.</span><span class="sxs-lookup"><span data-stu-id="9b914-133">See the language-specific sample that uses the input record from the binding.</span></span> <span data-ttu-id="9b914-134">A C# és F # minták is módosíthatja a rekord `text` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9b914-134">The C# and F# samples also modify the record's `text` property.</span></span>

* [<span data-ttu-id="9b914-135">C#</span><span class="sxs-lookup"><span data-stu-id="9b914-135">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="9b914-136">Node.js</span><span class="sxs-lookup"><span data-stu-id="9b914-136">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="9b914-137">A C# bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9b914-137">Input sample in C#</span></span> #

```cs
#r "Newtonsoft.Json"    
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, JObject record)
{
    if (record != null)
    {
        record["Text"] = "This has changed.";
    }    
}
```

<!--
<a name="inputfsharp"></a>
### Input sample in F# ## 

```fsharp
#r "Newtonsoft.Json"    
open Newtonsoft.Json.Linq
let Run(myQueueItem: string, record: JObject) =
  inputDocument?text <- "This has changed."
```
-->

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="9b914-138">A node.js bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="9b914-138">Input sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a><span data-ttu-id="9b914-139">Mobile Apps kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="9b914-139">Mobile Apps output binding</span></span>
<span data-ttu-id="9b914-140">Használja a Mobile Apps kimenete egy új rekordot írni a Mobile Apps tábla végpont kötése.</span><span class="sxs-lookup"><span data-stu-id="9b914-140">Use the Mobile Apps output binding to write a new record to a Mobile Apps table endpoint.</span></span>  

<span data-ttu-id="9b914-141">A Mobile Apps a függvényt használja a következő JSON-objektumot a kimeneti a `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="9b914-141">The Mobile Apps output for a function uses the following JSON object in the `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of output parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "out"
}
```

<span data-ttu-id="9b914-142">Vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="9b914-142">Note the following:</span></span>

* <span data-ttu-id="9b914-143">`connection`a függvény alkalmazásban, viszont a mobilalkalmazás URL-CÍMÉT tartalmazó Alkalmazásbeállítás nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="9b914-143">`connection` should contain the name of an app setting in your function app, which in turn contains the URL of your mobile app.</span></span> <span data-ttu-id="9b914-144">A funkció URL-címet a mobilalkalmazás elleni szükséges REST-műveletek összeállításához.</span><span class="sxs-lookup"><span data-stu-id="9b914-144">The function uses this URL to construct the required REST operations against your mobile app.</span></span> <span data-ttu-id="9b914-145">Akkor [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() a mobilalkalmazás URL-címet tartalmaz, amely (mely tűnik `http://<appname>.azurewebsites.net`), majd adja meg az Alkalmazásbeállítás a nevét a `connection` tulajdonságot a bemeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="9b914-145">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify the name of the app setting in the `connection` property in your input binding.</span></span> 
* <span data-ttu-id="9b914-146">Meg kell adnia `apiKey` Ha Ön [valósít meg API-kulcs a Node.js mobile Apps-háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), vagy [valósít meg API-kulcs a .NET-mobil háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="9b914-146">You need to specify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="9b914-147">Ehhez meg [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() , amely tartalmazza az API-kulcsot, majd adja hozzá a `apiKey` tulajdonságot a bemeneti kötése az Alkalmazásbeállítás nevét.</span><span class="sxs-lookup"><span data-stu-id="9b914-147">To do this, you [create an app setting in your function app]() that contains the API key, then add the `apiKey` property in your input binding with the name of the app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="9b914-148">Az API-kulcs nem kell osztani a mobilalkalmazás-ügyfelekkel.</span><span class="sxs-lookup"><span data-stu-id="9b914-148">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="9b914-149">Ez csak kell elosztani biztonságosan Szolgáltatásoldali ügyfelek, például az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="9b914-149">It should only be distributed securely to service-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="9b914-150">Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal.</span><span class="sxs-lookup"><span data-stu-id="9b914-150">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="9b914-151">Ez megvédi a bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="9b914-151">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="9b914-152">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="9b914-152">Output usage</span></span>
<span data-ttu-id="9b914-153">Ez a szakasz bemutatja, hogyan használhatja a Mobile Apps kimeneti kötelező a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="9b914-153">This section shows you how to use your Mobile Apps output binding in your function code.</span></span> 

<span data-ttu-id="9b914-154">A C# funkciók, használja a megnevezett kimeneti paramétert `out object` a kimeneti rekord eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9b914-154">In C# functions, use a named output parameter of type `out object` to access the output record.</span></span> <span data-ttu-id="9b914-155">A Node.js funkciók használata `context.bindings.<name>` a kimeneti rekord eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9b914-155">In Node.js functions, use `context.bindings.<name>` to access the output record.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="9b914-156">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="9b914-156">Output sample</span></span>
<span data-ttu-id="9b914-157">Tegyük fel, a következő function.json, amely meghatározza a várólista eseményindító és a Mobile Apps kimeneti:</span><span class="sxs-lookup"><span data-stu-id="9b914-157">Suppose you have the following function.json, that defines a queue trigger and a Mobile Apps output:</span></span>

```json
{
"bindings": [
    {
    "name": "myQueueItem",
    "queueName": "myqueue-items",
    "connection":"",
    "type": "queueTrigger",
    "direction": "in"
    },
    {
    "name": "record",
    "type": "mobileTable",
    "tableName": "MyTable",
    "connection": "My_MobileApp_Url",
    "apiKey": "My_MobileApp_Key",
    "direction": "out"
    }
],
"disabled": false
}
```

<span data-ttu-id="9b914-158">Tekintse meg a nyelvspecifikus mintát, amely létrehoz egy rekordot a Mobile Apps tábla végpont az üzenetsorban lévő üzenetet tartalmát.</span><span class="sxs-lookup"><span data-stu-id="9b914-158">See the language-specific sample that creates a record in the Mobile Apps table endpoint with the content of the queue message.</span></span>

* [<span data-ttu-id="9b914-159">C#</span><span class="sxs-lookup"><span data-stu-id="9b914-159">C#</span></span>](#outcsharp)
* [<span data-ttu-id="9b914-160">Node.js</span><span class="sxs-lookup"><span data-stu-id="9b914-160">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="9b914-161">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="9b914-161">Output sample in C#</span></span> #

```cs
public static void Run(string myQueueItem, out object record)
{
    record = new {
        Text = $"I'm running in a C# function! {myQueueItem}"
    };
}
```

<!--
<a name="outfsharp"></a>
### Output sample in F# ## 
```fsharp

```
-->
<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="9b914-162">Kimeneti minta node.js</span><span class="sxs-lookup"><span data-stu-id="9b914-162">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="9b914-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b914-163">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

