---
title: "aaaAzure funkciók Mobile Apps kötések |} Microsoft Docs"
description: "Megértéséhez hogyan toouse Azure Mobile Apps kötések Azure Functions."
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
ms.openlocfilehash: d3679a5d5c66705b32e422ec17e3a1e6d6ac063c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-mobile-apps-bindings"></a><span data-ttu-id="8fa4f-104">Az Azure Mobile Apps funkciók kötések</span><span class="sxs-lookup"><span data-stu-id="8fa4f-104">Azure Functions Mobile Apps bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="8fa4f-105">Ez a cikk azt ismerteti, hogyan tooconfigure és kód [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) Azure Functions kötések.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-105">This article explains how tooconfigure and code [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) bindings in Azure Functions.</span></span> <span data-ttu-id="8fa4f-106">Azure Functions támogatja bemeneti és kimeneti Mobile Apps kötéseit.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-106">Azure Functions supports input and output bindings for Mobile Apps.</span></span>

<span data-ttu-id="8fa4f-107">hello Mobile Apps bemeneti és kimeneti kötések lehetővé teszik, hogy [olvasási és írási toodata táblák](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) a mobilalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-107">hello Mobile Apps input and output bindings let you [read from and write toodata tables](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) in your mobile app.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a><span data-ttu-id="8fa4f-108">Mobile Apps bemeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8fa4f-108">Mobile Apps input binding</span></span>
<span data-ttu-id="8fa4f-109">Mobile Apps bemeneti kötése hello tölt be egy olyan rekordot egy mobil table végpont a, és továbbadja azt a függvényt.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-109">hello Mobile Apps input binding loads a record from a mobile table endpoint and passes it into your function.</span></span> <span data-ttu-id="8fa4f-110">A C# és F # függvényekben bármilyen változás toohello rekord automatikusan küldése hátsó toohello tábla amikor hello függvény sikeresen kilép.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-110">In a C# and F# functions, any changes made toohello record are automatically sent back toohello table when hello function exits successfully.</span></span>

<span data-ttu-id="8fa4f-111">Mobile Apps hello bemeneti tooa függvény használja a következő JSON-objektum a hello hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="8fa4f-111">hello Mobile Apps input tooa function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "mobileTable",
    "tableName": "<Name of your mobile app's data table>",
    "id" : "<Id of hello record tooretrieve - see below>",
    "connection": "<Name of app setting that has your mobile app's URL - see below>",
    "apiKey": "<Name of app setting that has your mobile app's API key - see below>",
    "direction": "in"
}
```

<span data-ttu-id="8fa4f-112">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="8fa4f-112">Note hello following:</span></span>

* <span data-ttu-id="8fa4f-113">`id`lehet, hogy statikus, vagy is alapulhat, amely meghívja hello függvényt hello eseményindító.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-113">`id` can be static, or it can be based on hello trigger that invokes hello function.</span></span> <span data-ttu-id="8fa4f-114">Például, ha egy [várólista eseményindító]() a függvénynél, majd `"id": "{queueTrigger}"` használ hello hello üzenetsor karakteres érték szerint rekord azonosítója tooretrieve hello.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-114">For example, if you use a [queue trigger]() for your function, then `"id": "{queueTrigger}"` uses hello string value of hello queue message as hello record ID tooretrieve.</span></span>
* <span data-ttu-id="8fa4f-115">`connection`a függvény alkalmazásban, viszont a mobilalkalmazás hello URL-CÍMÉT tartalmazó alkalmazás beállítása hello nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-115">`connection` should contain hello name of an app setting in your function app, which in turn contains hello URL of your mobile app.</span></span> <span data-ttu-id="8fa4f-116">a funkció hello a URL-cím tooconstruct hello szükséges REST műveleteinek a mobilalkalmazás ellen.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-116">hello function uses this URL tooconstruct hello required REST operations against your mobile app.</span></span> <span data-ttu-id="8fa4f-117">Akkor [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() a mobilalkalmazás URL-címet tartalmaz, amely (mely tűnik `http://<appname>.azurewebsites.net`), majd meg kell adnia hello nevet hello Alkalmazásbeállítás hello `connection` tulajdonságot a bemeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-117">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify hello name of hello app setting in hello `connection` property in your input binding.</span></span> 
* <span data-ttu-id="8fa4f-118">Toospecify kell `apiKey` Ha akkor [valósít meg API-kulcs a Node.js mobile Apps-háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), vagy [valósít meg API-kulcs a .NET-mobil háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="8fa4f-118">You need toospecify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="8fa4f-119">toodo, hogy [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() hello API-kulcsot tartalmazó, majd adja hozzá a hello `apiKey` tulajdonságot a bemeneti hello Alkalmazásbeállítás hello névhez történő kötés.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-119">toodo this, you [create an app setting in your function app]() that contains hello API key, then add hello `apiKey` property in your input binding with hello name of hello app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="8fa4f-120">Az API-kulcs nem kell osztani a mobilalkalmazás-ügyfelekkel.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-120">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="8fa4f-121">Elosztott biztonságosan tooservice mellett az ügyfelek, például az Azure Functions csak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-121">It should only be distributed securely tooservice-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="8fa4f-122">Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-122">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="8fa4f-123">Ez megvédi a bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-123">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="8fa4f-124">Bemeneti kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="8fa4f-124">Input usage</span></span>
<span data-ttu-id="8fa4f-125">Ez a szakasz bemutatja, hogyan toouse a Mobile Apps bemeneti kötelező a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-125">This section shows you how toouse your Mobile Apps input binding in your function code.</span></span> 

<span data-ttu-id="8fa4f-126">Amikor hello hello rekord megadott tábla és a rekordazonosító található, akkor átad a nevű hello [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) paraméter (vagy a node.js, akkor átad a hello `context.bindings.<name>` objektum).</span><span class="sxs-lookup"><span data-stu-id="8fa4f-126">When hello record with hello specified table and record ID is found, it is passed into hello named [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) parameter (or, in Node.js, it is passed into hello `context.bindings.<name>` object).</span></span> <span data-ttu-id="8fa4f-127">Hello rekord nem található, amikor hello paraméter `null`.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-127">When hello record is not found, hello parameter is `null`.</span></span> 

<span data-ttu-id="8fa4f-128">A C# és F # függvények toohello bemeneti rekord (bemeneti paraméter) végrehajtott módosítások automatikusan elküld hátsó toohello Mobile Apps tábla amikor hello függvény sikeresen kilép.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-128">In C# and F# functions, any changes you make toohello input record (input parameter) is automatically sent back toohello Mobile Apps table when hello function exits successfully.</span></span> <span data-ttu-id="8fa4f-129">A Node.js funkciók használata `context.bindings.<name>` tooaccess hello bemeneti rekord.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-129">In Node.js functions, use `context.bindings.<name>` tooaccess hello input record.</span></span> <span data-ttu-id="8fa4f-130">Node.js rekord nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-130">You cannot modify a record in Node.js.</span></span>

<a name="inputsample"></a>

## <a name="input-sample"></a><span data-ttu-id="8fa4f-131">A minta bemeneti</span><span class="sxs-lookup"><span data-stu-id="8fa4f-131">Input sample</span></span>
<span data-ttu-id="8fa4f-132">Tegyük fel, hogy rendelkezik a következő function.json hello, amely lekéri a Mobile Apps hello várólista eseményindító üzenet hello azonosítójú tábla egyik rekordja:</span><span class="sxs-lookup"><span data-stu-id="8fa4f-132">Suppose you have hello following function.json, that retrieves a Mobile App table record with hello id of hello queue trigger message:</span></span>

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

<span data-ttu-id="8fa4f-133">Lásd: a bemeneti rekord hello hello kötés használó hello nyelvspecifikus mintaalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-133">See hello language-specific sample that uses hello input record from hello binding.</span></span> <span data-ttu-id="8fa4f-134">hello C# és F # minták is módosíthatók hello rekord `text` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-134">hello C# and F# samples also modify hello record's `text` property.</span></span>

* [<span data-ttu-id="8fa4f-135">C#</span><span class="sxs-lookup"><span data-stu-id="8fa4f-135">C#</span></span>](#inputcsharp)
* [<span data-ttu-id="8fa4f-136">Node.js</span><span class="sxs-lookup"><span data-stu-id="8fa4f-136">Node.js</span></span>](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a><span data-ttu-id="8fa4f-137">A C# bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="8fa4f-137">Input sample in C#</span></span> #

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

### <a name="input-sample-in-nodejs"></a><span data-ttu-id="8fa4f-138">A node.js bemeneti minta</span><span class="sxs-lookup"><span data-stu-id="8fa4f-138">Input sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a><span data-ttu-id="8fa4f-139">Mobile Apps kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="8fa4f-139">Mobile Apps output binding</span></span>
<span data-ttu-id="8fa4f-140">Hello Mobile Apps kimeneti kötése toowrite új rekord tooa Mobile Apps tábla végpont használja.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-140">Use hello Mobile Apps output binding toowrite a new record tooa Mobile Apps table endpoint.</span></span>  

<span data-ttu-id="8fa4f-141">egy függvény hello hello a JSON-objektum a következő célokra kimeneti Mobile Apps hello `bindings` function.json tömbje:</span><span class="sxs-lookup"><span data-stu-id="8fa4f-141">hello Mobile Apps output for a function uses hello following JSON object in hello `bindings` array of function.json:</span></span>

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

<span data-ttu-id="8fa4f-142">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="8fa4f-142">Note hello following:</span></span>

* <span data-ttu-id="8fa4f-143">`connection`a függvény alkalmazásban, viszont a mobilalkalmazás hello URL-CÍMÉT tartalmazó alkalmazás beállítása hello nevét kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-143">`connection` should contain hello name of an app setting in your function app, which in turn contains hello URL of your mobile app.</span></span> <span data-ttu-id="8fa4f-144">a funkció hello a URL-cím tooconstruct hello szükséges REST műveleteinek a mobilalkalmazás ellen.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-144">hello function uses this URL tooconstruct hello required REST operations against your mobile app.</span></span> <span data-ttu-id="8fa4f-145">Akkor [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() a mobilalkalmazás URL-címet tartalmaz, amely (mely tűnik `http://<appname>.azurewebsites.net`), majd meg kell adnia hello nevet hello Alkalmazásbeállítás hello `connection` tulajdonságot a bemeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-145">You [create an app setting in your function app]() that contains your mobile app's URL (which looks like `http://<appname>.azurewebsites.net`), then specify hello name of hello app setting in hello `connection` property in your input binding.</span></span> 
* <span data-ttu-id="8fa4f-146">Toospecify kell `apiKey` Ha akkor [valósít meg API-kulcs a Node.js mobile Apps-háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), vagy [valósít meg API-kulcs a .NET-mobil háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span><span class="sxs-lookup"><span data-stu-id="8fa4f-146">You need toospecify `apiKey` if you [implement an API key in your Node.js mobile app backend](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), or [implement an API key in your .NET mobile app backend](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key).</span></span> <span data-ttu-id="8fa4f-147">toodo, hogy [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() hello API-kulcsot tartalmazó, majd adja hozzá a hello `apiKey` tulajdonságot a bemeneti hello Alkalmazásbeállítás hello névhez történő kötés.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-147">toodo this, you [create an app setting in your function app]() that contains hello API key, then add hello `apiKey` property in your input binding with hello name of hello app setting.</span></span> 
  
  > [!IMPORTANT]
  > <span data-ttu-id="8fa4f-148">Az API-kulcs nem kell osztani a mobilalkalmazás-ügyfelekkel.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-148">This API key must not be shared with your mobile app clients.</span></span> <span data-ttu-id="8fa4f-149">Elosztott biztonságosan tooservice mellett az ügyfelek, például az Azure Functions csak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-149">It should only be distributed securely tooservice-side clients, like Azure Functions.</span></span> 
  > 
  > [!NOTE]
  > <span data-ttu-id="8fa4f-150">Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-150">Azure Functions stores your connection information and API keys as app settings so that they are not checked into your source control repository.</span></span> <span data-ttu-id="8fa4f-151">Ez megvédi a bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-151">This safeguards your sensitive information.</span></span>
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="8fa4f-152">Kimeneti használata</span><span class="sxs-lookup"><span data-stu-id="8fa4f-152">Output usage</span></span>
<span data-ttu-id="8fa4f-153">Ez a szakasz bemutatja, hogyan toouse a Mobile Apps kimeneti kötelező a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-153">This section shows you how toouse your Mobile Apps output binding in your function code.</span></span> 

<span data-ttu-id="8fa4f-154">A C# funkciók, használja a megnevezett kimeneti paramétert `out object` tooaccess hello kimeneti rekord.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-154">In C# functions, use a named output parameter of type `out object` tooaccess hello output record.</span></span> <span data-ttu-id="8fa4f-155">A Node.js funkciók használata `context.bindings.<name>` tooaccess hello kimeneti rekord.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-155">In Node.js functions, use `context.bindings.<name>` tooaccess hello output record.</span></span>

<a name="outputsample"></a>

## <a name="output-sample"></a><span data-ttu-id="8fa4f-156">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="8fa4f-156">Output sample</span></span>
<span data-ttu-id="8fa4f-157">Tegyük fel, a következő function.json, amely meghatározza a várólista eseményindító és a Mobile Apps kimeneti hello:</span><span class="sxs-lookup"><span data-stu-id="8fa4f-157">Suppose you have hello following function.json, that defines a queue trigger and a Mobile Apps output:</span></span>

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

<span data-ttu-id="8fa4f-158">Lásd: hello nyelvspecifikus mintát, amely létrehoz egy rekordot hello Mobile Apps tábla végpont hello üzenetsor hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="8fa4f-158">See hello language-specific sample that creates a record in hello Mobile Apps table endpoint with hello content of hello queue message.</span></span>

* [<span data-ttu-id="8fa4f-159">C#</span><span class="sxs-lookup"><span data-stu-id="8fa4f-159">C#</span></span>](#outcsharp)
* [<span data-ttu-id="8fa4f-160">Node.js</span><span class="sxs-lookup"><span data-stu-id="8fa4f-160">Node.js</span></span>](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a><span data-ttu-id="8fa4f-161">A C# kimeneti minta</span><span class="sxs-lookup"><span data-stu-id="8fa4f-161">Output sample in C#</span></span> #

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

### <a name="output-sample-in-nodejs"></a><span data-ttu-id="8fa4f-162">Kimeneti minta node.js</span><span class="sxs-lookup"><span data-stu-id="8fa4f-162">Output sample in Node.js</span></span>

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="8fa4f-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8fa4f-163">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

