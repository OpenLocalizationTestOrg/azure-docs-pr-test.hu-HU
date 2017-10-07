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
# <a name="azure-functions-mobile-apps-bindings"></a>Az Azure Mobile Apps funkciók kötések
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Ez a cikk azt ismerteti, hogyan tooconfigure és kód [Azure Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) Azure Functions kötések. Azure Functions támogatja bemeneti és kimeneti Mobile Apps kötéseit.

hello Mobile Apps bemeneti és kimeneti kötések lehetővé teszik, hogy [olvasási és írási toodata táblák](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) a mobilalkalmazásban.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="mobile-apps-input-binding"></a>Mobile Apps bemeneti kötése
Mobile Apps bemeneti kötése hello tölt be egy olyan rekordot egy mobil table végpont a, és továbbadja azt a függvényt. A C# és F # függvényekben bármilyen változás toohello rekord automatikusan küldése hátsó toohello tábla amikor hello függvény sikeresen kilép.

Mobile Apps hello bemeneti tooa függvény használja a következő JSON-objektum a hello hello `bindings` function.json tömbje:

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

Vegye figyelembe a következőket hello:

* `id`lehet, hogy statikus, vagy is alapulhat, amely meghívja hello függvényt hello eseményindító. Például, ha egy [várólista eseményindító]() a függvénynél, majd `"id": "{queueTrigger}"` használ hello hello üzenetsor karakteres érték szerint rekord azonosítója tooretrieve hello.
* `connection`a függvény alkalmazásban, viszont a mobilalkalmazás hello URL-CÍMÉT tartalmazó alkalmazás beállítása hello nevét kell tartalmaznia. a funkció hello a URL-cím tooconstruct hello szükséges REST műveleteinek a mobilalkalmazás ellen. Akkor [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() a mobilalkalmazás URL-címet tartalmaz, amely (mely tűnik `http://<appname>.azurewebsites.net`), majd meg kell adnia hello nevet hello Alkalmazásbeállítás hello `connection` tulajdonságot a bemeneti kötése. 
* Toospecify kell `apiKey` Ha akkor [valósít meg API-kulcs a Node.js mobile Apps-háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), vagy [valósít meg API-kulcs a .NET-mobil háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). toodo, hogy [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() hello API-kulcsot tartalmazó, majd adja hozzá a hello `apiKey` tulajdonságot a bemeneti hello Alkalmazásbeállítás hello névhez történő kötés. 
  
  > [!IMPORTANT]
  > Az API-kulcs nem kell osztani a mobilalkalmazás-ügyfelekkel. Elosztott biztonságosan tooservice mellett az ügyfelek, például az Azure Functions csak kell lennie. 
  > 
  > [!NOTE]
  > Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal. Ez megvédi a bizalmas adatokat.
  > 
  > 

<a name="inputusage"></a>

## <a name="input-usage"></a>Bemeneti kihasználtsága
Ez a szakasz bemutatja, hogyan toouse a Mobile Apps bemeneti kötelező a funkciókódot. 

Amikor hello hello rekord megadott tábla és a rekordazonosító található, akkor átad a nevű hello [JObject](http://www.newtonsoft.com/json/help/html/t_newtonsoft_json_linq_jobject.htm) paraméter (vagy a node.js, akkor átad a hello `context.bindings.<name>` objektum). Hello rekord nem található, amikor hello paraméter `null`. 

A C# és F # függvények toohello bemeneti rekord (bemeneti paraméter) végrehajtott módosítások automatikusan elküld hátsó toohello Mobile Apps tábla amikor hello függvény sikeresen kilép. A Node.js funkciók használata `context.bindings.<name>` tooaccess hello bemeneti rekord. Node.js rekord nem módosítható.

<a name="inputsample"></a>

## <a name="input-sample"></a>A minta bemeneti
Tegyük fel, hogy rendelkezik a következő function.json hello, amely lekéri a Mobile Apps hello várólista eseményindító üzenet hello azonosítójú tábla egyik rekordja:

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

Lásd: a bemeneti rekord hello hello kötés használó hello nyelvspecifikus mintaalkalmazás. hello C# és F # minták is módosíthatók hello rekord `text` tulajdonság.

* [C#](#inputcsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>A C# bemeneti minta #

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

### <a name="input-sample-in-nodejs"></a>A node.js bemeneti minta

```javascript
module.exports = function (context, myQueueItem) {    
    context.log(context.bindings.record);
    context.done();
};
```

<a name="output"></a>

## <a name="mobile-apps-output-binding"></a>Mobile Apps kimeneti kötése
Hello Mobile Apps kimeneti kötése toowrite új rekord tooa Mobile Apps tábla végpont használja.  

egy függvény hello hello a JSON-objektum a következő célokra kimeneti Mobile Apps hello `bindings` function.json tömbje:

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

Vegye figyelembe a következőket hello:

* `connection`a függvény alkalmazásban, viszont a mobilalkalmazás hello URL-CÍMÉT tartalmazó alkalmazás beállítása hello nevét kell tartalmaznia. a funkció hello a URL-cím tooconstruct hello szükséges REST műveleteinek a mobilalkalmazás ellen. Akkor [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() a mobilalkalmazás URL-címet tartalmaz, amely (mely tűnik `http://<appname>.azurewebsites.net`), majd meg kell adnia hello nevet hello Alkalmazásbeállítás hello `connection` tulajdonságot a bemeneti kötése. 
* Toospecify kell `apiKey` Ha akkor [valósít meg API-kulcs a Node.js mobile Apps-háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/api-key), vagy [valósít meg API-kulcs a .NET-mobil háttéralkalmazás](https://github.com/Azure/azure-mobile-apps-net-server/wiki/Implementing-Application-Key). toodo, hogy [alkalmazásbeállítás létrehozása az függvény alkalmazásban]() hello API-kulcsot tartalmazó, majd adja hozzá a hello `apiKey` tulajdonságot a bemeneti hello Alkalmazásbeállítás hello névhez történő kötés. 
  
  > [!IMPORTANT]
  > Az API-kulcs nem kell osztani a mobilalkalmazás-ügyfelekkel. Elosztott biztonságosan tooservice mellett az ügyfelek, például az Azure Functions csak kell lennie. 
  > 
  > [!NOTE]
  > Az Azure Functions tárolja a kapcsolati adatokat és API-kulcsokat beállításait, hogy nem ellenőrzi azokat a verziókövetési tárházzal. Ez megvédi a bizalmas adatokat.
  > 
  > 

<a name="outputusage"></a>

## <a name="output-usage"></a>Kimeneti használata
Ez a szakasz bemutatja, hogyan toouse a Mobile Apps kimeneti kötelező a funkciókódot. 

A C# funkciók, használja a megnevezett kimeneti paramétert `out object` tooaccess hello kimeneti rekord. A Node.js funkciók használata `context.bindings.<name>` tooaccess hello kimeneti rekord.

<a name="outputsample"></a>

## <a name="output-sample"></a>Minta kimenet
Tegyük fel, a következő function.json, amely meghatározza a várólista eseményindító és a Mobile Apps kimeneti hello:

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

Lásd: hello nyelvspecifikus mintát, amely létrehoz egy rekordot hello Mobile Apps tábla végpont hello üzenetsor hello tartalmát.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>A C# kimeneti minta #

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

### <a name="output-sample-in-nodejs"></a>Kimeneti minta node.js

```javascript
module.exports = function (context, myQueueItem) {

    context.bindings.record = {
        text : "I'm running in a Node function! Data: '" + myQueueItem + "'"
    }   

    context.done();
};
```

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

