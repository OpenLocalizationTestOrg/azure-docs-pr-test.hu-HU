---
title: "az eseményindítók és kötések az Azure Functions aaaWork |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse eseményindítók és kötések az Azure Functions tooconnect a kód végrehajtása tooonline események és a felhő alapú szolgáltatások."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: cbc7460a-4d8a-423f-a63e-1cd33fef7252
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: donnam
ms.openlocfilehash: eb2ebfca172fcc8c0f479adbcfec99e90fc33615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a><span data-ttu-id="3d970-104">Az Azure Functions eseményindítók és kötések fogalmak</span><span class="sxs-lookup"><span data-stu-id="3d970-104">Azure Functions triggers and bindings concepts</span></span>
<span data-ttu-id="3d970-105">Az Azure Functions lehetővé teszi az Azure-ban és egyéb szolgáltatások válasz tooevents toowrite kód keresztül *eseményindítók* és *kötések*.</span><span class="sxs-lookup"><span data-stu-id="3d970-105">Azure Functions allows you toowrite code in response tooevents in Azure and other services, through *triggers* and *bindings*.</span></span> <span data-ttu-id="3d970-106">Ez a cikk eseményindítók elméleti áttekintését és kötések az összes támogatott programozási nyelveket.</span><span class="sxs-lookup"><span data-stu-id="3d970-106">This article is a conceptual overview of triggers and bindings for all supported programming languages.</span></span> <span data-ttu-id="3d970-107">Szolgáltatások közös tooall kötések is itt részelemcímkék ismertetését.</span><span class="sxs-lookup"><span data-stu-id="3d970-107">Features that are common tooall bindings are described here.</span></span>

## <a name="overview"></a><span data-ttu-id="3d970-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3d970-108">Overview</span></span>

<span data-ttu-id="3d970-109">Eseményindítók és kötések egy deklaratív módon toodefine hogyan függvényt hívják, és milyen adatokat is működik.</span><span class="sxs-lookup"><span data-stu-id="3d970-109">Triggers and bindings are a declarative way toodefine how a function is invoked and what data it works with.</span></span> <span data-ttu-id="3d970-110">A *eseményindító* határozza meg, hogyan függvényt hívják.</span><span class="sxs-lookup"><span data-stu-id="3d970-110">A *trigger* defines how a function is invoked.</span></span> <span data-ttu-id="3d970-111">A függvénynek pontosan egy eseményindító kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3d970-111">A function must have exactly one trigger.</span></span> <span data-ttu-id="3d970-112">Eseményindítók olyan adatok, amely általában hello függvény kiváltó hello hasznos lehet társítva.</span><span class="sxs-lookup"><span data-stu-id="3d970-112">Triggers have associated data, which is usually hello payload that triggered hello function.</span></span> 

<span data-ttu-id="3d970-113">Bemeneti és kimeneti *kötések* adjon meg egy deklaratív módon tooconnect toodata, az a kód.</span><span class="sxs-lookup"><span data-stu-id="3d970-113">Input and output *bindings* provide a declarative way tooconnect toodata from within your code.</span></span> <span data-ttu-id="3d970-114">Hasonló tootriggers, adja meg a kapcsolati karakterláncokat és egyéb tulajdonságai a függvény konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="3d970-114">Similar tootriggers, you specify connection strings and other properties in your function configuration.</span></span> <span data-ttu-id="3d970-115">Kötések nem kötelező, és egy függvény több bemeneti és a kimeneti kötéseket.</span><span class="sxs-lookup"><span data-stu-id="3d970-115">Bindings are optional and a function can have multiple input and output bindings.</span></span> 

<span data-ttu-id="3d970-116">Az eseményindítók és kötések, írhat kódot, amely több általános, és amelyen nem kódba foglalni hello részletek hello szolgáltatás, amellyel kommunikál.</span><span class="sxs-lookup"><span data-stu-id="3d970-116">Using triggers and bindings, you can write code that is more generic and does not hardcode hello details of hello services with which it interacts.</span></span> <span data-ttu-id="3d970-117">Egyszerűen vált szolgáltatások bemeneti értékeket a funkciókódot érkező adatokat.</span><span class="sxs-lookup"><span data-stu-id="3d970-117">Data coming from services simply become input values for your function code.</span></span> <span data-ttu-id="3d970-118">tooanother adatszolgáltatás toooutput (például létrehozhat egy új sor Azure Table Storage-ban), használja a hello hello metódus visszatérési értéke.</span><span class="sxs-lookup"><span data-stu-id="3d970-118">toooutput data tooanother service (such as creating a new row in Azure Table Storage), use hello return value of hello method.</span></span> <span data-ttu-id="3d970-119">Vagy ha több értéket kell toooutput, akkor egy segítő objektuma.</span><span class="sxs-lookup"><span data-stu-id="3d970-119">Or, if you need toooutput multiple values, use a helper object.</span></span> <span data-ttu-id="3d970-120">Eseményindítók és kötések rendelkezik egy **neve** tulajdonságot, amelynek segítségével a kód tooaccess hello kötés azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3d970-120">Triggers and bindings have a **name** property, which is an identifier you use in your code tooaccess hello binding.</span></span>

<span data-ttu-id="3d970-121">Eseményindítók és kötések konfigurálható hello **integráció** hello Azure Functions portálon lapon.</span><span class="sxs-lookup"><span data-stu-id="3d970-121">You can configure triggers and bindings in hello **Integrate** tab in hello Azure Functions portal.</span></span> <span data-ttu-id="3d970-122">Hello színfalak hello felhasználói felület módosít egy fájlt, úgynevezett *function.json* hello függvény fájl.</span><span class="sxs-lookup"><span data-stu-id="3d970-122">Under hello covers, hello UI modifies a file called *function.json* file in hello function directory.</span></span> <span data-ttu-id="3d970-123">Ez a fájl szerkesztésével toohello módosítása **speciális szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="3d970-123">You can edit this file by changing toohello **Advanced editor**.</span></span>

<span data-ttu-id="3d970-124">hello következő táblázatban hello eseményindítók és kötések, az Azure Functions által támogatott.</span><span class="sxs-lookup"><span data-stu-id="3d970-124">hello following table shows hello triggers and bindings that are supported with Azure Functions.</span></span> 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a><span data-ttu-id="3d970-125">Példa: a várólista eseményindító és tábla kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="3d970-125">Example: queue trigger and table output binding</span></span>

<span data-ttu-id="3d970-126">Tegyük fel, hogy egy új sor tooAzure Table Storage toowrite szüksége, amikor az Azure Queue Storage egy új üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3d970-126">Suppose you want toowrite a new row tooAzure Table Storage whenever a new message appears in Azure Queue Storage.</span></span> <span data-ttu-id="3d970-127">Ebben a forgatókönyvben az Azure Queue valósítható eseményindító és egy tábla kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="3d970-127">This scenario can be implemented using an Azure Queue trigger and a Table output binding.</span></span> 

<span data-ttu-id="3d970-128">Várólista eseményindító igényel a következő információk hello hello **integráció** lapon:</span><span class="sxs-lookup"><span data-stu-id="3d970-128">A queue trigger requires hello following information in hello **Integrate** tab:</span></span>

* <span data-ttu-id="3d970-129">hello hello tárolási fiók kapcsolati karakterlánc hello várólista tartalmazó hello alkalmazásbeállítás neve</span><span class="sxs-lookup"><span data-stu-id="3d970-129">hello name of hello app setting that contains hello storage account connection string for hello queue</span></span>
* <span data-ttu-id="3d970-130">hello várólista neve</span><span class="sxs-lookup"><span data-stu-id="3d970-130">hello queue name</span></span>
* <span data-ttu-id="3d970-131">a kód tooread hello tartalmában várólista üdvözlőüzenetére azonosítója, mint hello `order`.</span><span class="sxs-lookup"><span data-stu-id="3d970-131">hello identifier in your code tooread hello contents of hello queue message, such as `order`.</span></span>

<span data-ttu-id="3d970-132">toowrite tooAzure Table Storage egy kimeneti kötése a következő adatok hello használata:</span><span class="sxs-lookup"><span data-stu-id="3d970-132">toowrite tooAzure Table Storage, use an output binding with hello following details:</span></span>

* <span data-ttu-id="3d970-133">hello hello tárolási fiók kapcsolati karakterlánc hello tábla tartalmazó hello alkalmazásbeállítás neve</span><span class="sxs-lookup"><span data-stu-id="3d970-133">hello name of hello app setting that contains hello storage account connection string for hello table</span></span>
* <span data-ttu-id="3d970-134">hello tábla neve</span><span class="sxs-lookup"><span data-stu-id="3d970-134">hello table name</span></span>
* <span data-ttu-id="3d970-135">a kód toocreate hello azonosító kimeneti elemek vagy hello visszatérési érték a hello függvény.</span><span class="sxs-lookup"><span data-stu-id="3d970-135">hello identifier in your code toocreate output items, or hello return value from hello function.</span></span>

<span data-ttu-id="3d970-136">Kapcsolati karakterláncok tooenforce hello bevált gyakorlat az, hogy inkább kötések Alkalmazásbeállítások *function.json* szolgáltatás titkos kulcsokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3d970-136">Bindings use app settings for connection strings tooenforce hello best practice that *function.json* does not contain service secrets.</span></span>

<span data-ttu-id="3d970-137">Ezt követően a toointegrate az Azure Storage a kódban megadott hello-azonosítók használata.</span><span class="sxs-lookup"><span data-stu-id="3d970-137">Then, use hello identifiers you provided toointegrate with Azure Storage in your code.</span></span>

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write tooTable Storage
// hello method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

```javascript
// From an incoming queue message that is a JSON object, add fields and write tooTable Storage
// hello second parameter toocontext.done is used as hello value for hello new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

<span data-ttu-id="3d970-138">Íme hello *function.json* , amely megfelel a kód megelőző toohello.</span><span class="sxs-lookup"><span data-stu-id="3d970-138">Here is hello *function.json* that corresponds toohello preceding code.</span></span> <span data-ttu-id="3d970-139">Vegye figyelembe, hogy hello azonos konfigurációval is használható, függetlenül hello függvény végrehajtása hello nyelvét.</span><span class="sxs-lookup"><span data-stu-id="3d970-139">Note that hello same configuration can be used, regardless of hello language of hello function implementation.</span></span>

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```
<span data-ttu-id="3d970-140">tooview és Szerkesztés hello tartalmát *function.json* hello Azure-portálon, kattintson a hello **speciális szerkesztő** hello beállítást **integráció** lapon, a függvény.</span><span class="sxs-lookup"><span data-stu-id="3d970-140">tooview and edit hello contents of *function.json* in hello Azure portal, click hello **Advanced editor** option on hello **Integrate** tab of your function.</span></span>

<span data-ttu-id="3d970-141">További példákat és részleteinek integrálása az Azure Storage: [Azure Functions eseményindítók és kötések az Azure Storage](functions-bindings-storage.md).</span><span class="sxs-lookup"><span data-stu-id="3d970-141">For more code examples and details on integrating with Azure Storage, see [Azure Functions triggers and bindings for Azure Storage](functions-bindings-storage.md).</span></span>

### <a name="binding-direction"></a><span data-ttu-id="3d970-142">Kötési iránya</span><span class="sxs-lookup"><span data-stu-id="3d970-142">Binding direction</span></span>

<span data-ttu-id="3d970-143">Az összes eseményindítók és kötések vannak egy `direction` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="3d970-143">All triggers and bindings have a `direction` property:</span></span>

- <span data-ttu-id="3d970-144">Az eseményindítók hello irány mindig van kapcsolva`in`</span><span class="sxs-lookup"><span data-stu-id="3d970-144">For triggers, hello direction is always `in`</span></span>
- <span data-ttu-id="3d970-145">Bemeneti és kimeneti kötések használhatják `in` és`out`</span><span class="sxs-lookup"><span data-stu-id="3d970-145">Input and output bindings use `in` and `out`</span></span>
- <span data-ttu-id="3d970-146">Néhány kötések támogatja a speciális paraméterirányt `inout`.</span><span class="sxs-lookup"><span data-stu-id="3d970-146">Some bindings support a special direction `inout`.</span></span> <span data-ttu-id="3d970-147">Ha `inout`, csak hello **speciális szerkesztő** érhető el hello **integráció** lapon.</span><span class="sxs-lookup"><span data-stu-id="3d970-147">If you use `inout`, only hello **Advanced editor** is available in hello **Integrate** tab.</span></span>

## <a name="using-hello-function-return-type-tooreturn-a-single-output"></a><span data-ttu-id="3d970-148">Hello függvény visszatérési típusa tooreturn egyetlen kimeneti használatával</span><span class="sxs-lookup"><span data-stu-id="3d970-148">Using hello function return type tooreturn a single output</span></span>

<span data-ttu-id="3d970-149">hello előző példa bemutatja, hogyan toouse hello függvény visszatérési értéke tooprovide kimeneti tooa kötést, amely speciális hello paraméter használatával `$return`.</span><span class="sxs-lookup"><span data-stu-id="3d970-149">hello preceding example shows how toouse hello function return value tooprovide output tooa binding, which is achieved by using hello special name parameter `$return`.</span></span> <span data-ttu-id="3d970-150">(Ez csak akkor támogatott a nyelveket, amelyeken a visszatérési érték, például a C#, JavaScript és F #.) Ha egy függvény több kimeneti kötése, `$return` hello kimeneti kötések csak az egyik.</span><span class="sxs-lookup"><span data-stu-id="3d970-150">(This is only supported in languages that have a return value, such as C#, JavaScript, and F#.) If a function has multiple output bindings, use `$return` for only one of hello output bindings.</span></span> 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

<span data-ttu-id="3d970-151">alább megjelenítése hello példák hogyan visszatérési típusok használhatók kimeneti kötések C#, JavaScript és F #.</span><span class="sxs-lookup"><span data-stu-id="3d970-151">hello examples below show how return types are used with output bindings in C#, JavaScript, and F#.</span></span>

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```javascript
// JavaScript: return a value in hello second parameter toocontext.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a><span data-ttu-id="3d970-152">Kötés dataType tulajdonsága</span><span class="sxs-lookup"><span data-stu-id="3d970-152">Binding dataType property</span></span>

<span data-ttu-id="3d970-153">A .NET a bemeneti adatok a hello típusok toodefine hello adattípust használja.</span><span class="sxs-lookup"><span data-stu-id="3d970-153">In .NET, use hello types toodefine hello data type for input data.</span></span> <span data-ttu-id="3d970-154">Használja például a `string` toobind toohello szöveg várólista eseményindító és egy bájt tömb tooread bináris formában.</span><span class="sxs-lookup"><span data-stu-id="3d970-154">For instance, use `string` toobind toohello text of a queue trigger and a byte array tooread as binary.</span></span>

<span data-ttu-id="3d970-155">A dinamikusan begépelt például JavaScript nyelven, használja a hello `dataType` hello kötés definícióban tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3d970-155">For languages that are dynamically typed such as JavaScript, use hello `dataType` property in hello binding definition.</span></span> <span data-ttu-id="3d970-156">Például tooread hello bináris formátumú HTTP-kérelem tartalma, hello típust használjon `binary`:</span><span class="sxs-lookup"><span data-stu-id="3d970-156">For example, tooread hello content of an HTTP request in binary format, use hello type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="3d970-157">Más beállításokat a `dataType` vannak `stream` és `string`.</span><span class="sxs-lookup"><span data-stu-id="3d970-157">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="resolving-app-settings"></a><span data-ttu-id="3d970-158">Alkalmazásbeállítások feloldása</span><span class="sxs-lookup"><span data-stu-id="3d970-158">Resolving app settings</span></span>
<span data-ttu-id="3d970-159">Ajánlott eljárásként titkos kulcsok és a kapcsolati karakterláncok használatával kell irányítani Alkalmazásbeállítások, nem pedig konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="3d970-159">As a best practice, secrets and connection strings should be managed using app settings, rather than configuration files.</span></span> <span data-ttu-id="3d970-160">Ez korlátozza a hozzáférést toothese titkos kulcsokat, és lehetővé teszi az biztonságos toostore *function.json* a egy nyilvános verziókövetési tárházzal.</span><span class="sxs-lookup"><span data-stu-id="3d970-160">This limits access toothese secrets and makes it safe toostore *function.json* in a public source control repository.</span></span>

<span data-ttu-id="3d970-161">Alkalmazásbeállítások is hasznosak, ha azt szeretné, hogy a hello környezete alapján toochange konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="3d970-161">App settings are also useful whenever you want toochange configuration based on hello environment.</span></span> <span data-ttu-id="3d970-162">Például egy tesztkörnyezetben, érdemes lehet toomonitor különböző várólista vagy a blob storage tárolót.</span><span class="sxs-lookup"><span data-stu-id="3d970-162">For example, in a test environment, you may want toomonitor a different queue or blob storage container.</span></span>

<span data-ttu-id="3d970-163">Alkalmazásbeállítások fakadó problémák megoldásával, amikor egy érték szimpla százalékjelek, például a `%MyAppSetting%`.</span><span class="sxs-lookup"><span data-stu-id="3d970-163">App settings are resolved whenever a value is enclosed in percent signs, such as `%MyAppSetting%`.</span></span> <span data-ttu-id="3d970-164">Vegye figyelembe, hogy hello `connection` eseményindítók és kötések tulajdonsága egy különleges esetben, és automatikusan feloldja az értékeket, ha az alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="3d970-164">Note that hello `connection` property of triggers and bindings is a special case and automatically resolves values as app settings.</span></span> 

<span data-ttu-id="3d970-165">hello alábbi példa: egy sor eseményindító Alkalmazásbeállítás használó `%input-queue-name%` toodefine hello várólista tootrigger meg.</span><span class="sxs-lookup"><span data-stu-id="3d970-165">hello following example is a queue trigger that uses an app setting `%input-queue-name%` toodefine hello queue tootrigger on.</span></span>

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

## <a name="trigger-metadata-properties"></a><span data-ttu-id="3d970-166">Eseményindító metaadat-tulajdonságainak</span><span class="sxs-lookup"><span data-stu-id="3d970-166">Trigger metadata properties</span></span>

<span data-ttu-id="3d970-167">Sok eseményindítók hozzáadása a trigger (például egy olyan függvényt kiváltó hello üzenetsor) által biztosított toohello adatokat tartalmaz, adja meg a további metaadatokat értékét.</span><span class="sxs-lookup"><span data-stu-id="3d970-167">In addition toohello data payload provided by a trigger (such as hello queue message that triggered a function), many triggers provide additional metadata values.</span></span> <span data-ttu-id="3d970-168">Ezeket az értékeket a C# és F # vagy hello tulajdonságainak bemeneti paraméter használható `context.bindings` JavaScript objektumban.</span><span class="sxs-lookup"><span data-stu-id="3d970-168">These values can be used as input parameters in C# and F# or properties on hello `context.bindings` object in JavaScript.</span></span> 

<span data-ttu-id="3d970-169">Például egy várólista eseményindító támogatja hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="3d970-169">For example, a queue trigger supports hello following properties:</span></span>

* <span data-ttu-id="3d970-170">QueueTrigger - indítására üzenet tartalmát, ha egy érvényes karakterláncot</span><span class="sxs-lookup"><span data-stu-id="3d970-170">QueueTrigger - triggering message content if a valid string</span></span>
* <span data-ttu-id="3d970-171">DequeueCount</span><span class="sxs-lookup"><span data-stu-id="3d970-171">DequeueCount</span></span>
* <span data-ttu-id="3d970-172">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="3d970-172">ExpirationTime</span></span>
* <span data-ttu-id="3d970-173">Azonosító</span><span class="sxs-lookup"><span data-stu-id="3d970-173">Id</span></span>
* <span data-ttu-id="3d970-174">InsertionTime</span><span class="sxs-lookup"><span data-stu-id="3d970-174">InsertionTime</span></span>
* <span data-ttu-id="3d970-175">NextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="3d970-175">NextVisibleTime</span></span>
* <span data-ttu-id="3d970-176">PopReceipt</span><span class="sxs-lookup"><span data-stu-id="3d970-176">PopReceipt</span></span>

<span data-ttu-id="3d970-177">Hello megfelelő referencia-témakör ismerteti a metaadat-tulajdonságainak minden eseményindító részleteit.</span><span class="sxs-lookup"><span data-stu-id="3d970-177">Details of metadata properties for each trigger are described in hello corresponding reference topic.</span></span> <span data-ttu-id="3d970-178">Dokumentáció is rendelkezésre áll, a hello **integráció** hello portál, a hello lapján **dokumentáció** hello kötés konfigurációs terület szakaszban olvashatók.</span><span class="sxs-lookup"><span data-stu-id="3d970-178">Documentation is also available in hello **Integrate** tab of hello portal, in hello **Documentation** section below hello binding configuration area.</span></span>  

<span data-ttu-id="3d970-179">Például, mivel a blob eseményindítók késedelmes rendelkeznek, egy várólista eseményindító toorun a funkció használata (lásd: [Blob Storage eseményindító](functions-bindings-storage-blob.md#storage-blob-trigger).</span><span class="sxs-lookup"><span data-stu-id="3d970-179">For example, since blob triggers have some delays, you can use a queue trigger toorun your function (see [Blob Storage Trigger](functions-bindings-storage-blob.md#storage-blob-trigger).</span></span> <span data-ttu-id="3d970-180">várólista üdvözlőüzenetére hello blob fájlnév tootrigger tartalmazná a.</span><span class="sxs-lookup"><span data-stu-id="3d970-180">hello queue message would contain hello blob filename tootrigger on.</span></span> <span data-ttu-id="3d970-181">Hello segítségével `queueTrigger` metaadat-tulajdonságnak adhat meg ezt a viselkedést összes konfigurációjáról, nem pedig a kódot.</span><span class="sxs-lookup"><span data-stu-id="3d970-181">Using hello `queueTrigger` metadata property, you can specify this behavior all in your configuration, rather than your code.</span></span>

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

<span data-ttu-id="3d970-182">Egy metaadat-tulajdonságot is használható egy *kötési kifejezés* egy másik kötés, mint a következő szakaszban ismertetett hello.</span><span class="sxs-lookup"><span data-stu-id="3d970-182">Metadata properties from a trigger can also be used in a *binding expression* for another binding, as described in hello following section.</span></span>

## <a name="binding-expressions-and-patterns"></a><span data-ttu-id="3d970-183">Kötelező kifejezések és minták</span><span class="sxs-lookup"><span data-stu-id="3d970-183">Binding expressions and patterns</span></span>

<span data-ttu-id="3d970-184">Egyik leghatékonyabb szolgáltatása hello eseményindítók és kötések *kötési kifejezésként*.</span><span class="sxs-lookup"><span data-stu-id="3d970-184">One of hello most powerful features of triggers and bindings is *binding expressions*.</span></span> <span data-ttu-id="3d970-185">A kötés belül mintát kifejezések, amelyek ezután felhasználhatók adhat meg más kötésekben vagy a kód.</span><span class="sxs-lookup"><span data-stu-id="3d970-185">Within your binding, you can define pattern expressions which can then be used in other bindings or your code.</span></span> <span data-ttu-id="3d970-186">Eseményindító metaadatait is használható a kötési kifejezésként, mint az előző szakaszban hello hello mintában megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="3d970-186">Trigger metadata can also be used in binding expressions, as show in hello sample in hello preceding section.</span></span>

<span data-ttu-id="3d970-187">Tegyük fel például, azt szeretné, hogy az adott blob storage tárolót, hasonló toohello tooresize képek **kép méret** hello sablon **új függvény** lap.</span><span class="sxs-lookup"><span data-stu-id="3d970-187">For example, suppose you want tooresize images in particular blob storage container, similar toohello **Image Resizer** template in hello **New Function** page.</span></span> <span data-ttu-id="3d970-188">Nyissa meg túl**új függvény** -> nyelvi **C#** forgatókönyv -> **minták** -> **ImageResizer-c Sharp**.</span><span class="sxs-lookup"><span data-stu-id="3d970-188">Go too**New Function** -> Language **C#** -> Scenario **Samples** -> **ImageResizer-CSharp**.</span></span> 

<span data-ttu-id="3d970-189">Íme hello *function.json* definíciója:</span><span class="sxs-lookup"><span data-stu-id="3d970-189">Here is hello *function.json* definition:</span></span>

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

<span data-ttu-id="3d970-190">Figyelje meg, hogy hello `filename` paraméter van megadva a egyaránt hello blob eseményindító definícióját, valamint hello blob kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="3d970-190">Notice that hello `filename` parameter is used in both hello blob trigger definition as well as hello blob output binding.</span></span> <span data-ttu-id="3d970-191">Ez a paraméter funkciókódot is használható.</span><span class="sxs-lookup"><span data-stu-id="3d970-191">This parameter can also be used in function code.</span></span>

```csharp
// C# example of binding too{filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a><span data-ttu-id="3d970-192">Véletlenszerű GUID azonosítók</span><span class="sxs-lookup"><span data-stu-id="3d970-192">Random GUIDs</span></span>
<span data-ttu-id="3d970-193">Az Azure Functions kényelmi szintaxist tartalmaz a kötések hello keresztül a GUID-EK létrehozásának `{rand-guid}` kötési kifejezés.</span><span class="sxs-lookup"><span data-stu-id="3d970-193">Azure Functions provides a convenience syntax for generating GUIDs in your bindings, through hello `{rand-guid}` binding expression.</span></span> <span data-ttu-id="3d970-194">hello következő példában a toogenerate egy egyedi blob neve:</span><span class="sxs-lookup"><span data-stu-id="3d970-194">hello following example uses this toogenerate a unique blob name:</span></span> 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a><span data-ttu-id="3d970-195">Aktuális idő</span><span class="sxs-lookup"><span data-stu-id="3d970-195">Current time</span></span>

<span data-ttu-id="3d970-196">Hello kötési kifejezés használható `DateTime`, amely feloldja túl`DateTime.UtcNow`.</span><span class="sxs-lookup"><span data-stu-id="3d970-196">You can use hello binding expression `DateTime`, which resolves too`DateTime.UtcNow`.</span></span>

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-toocustom-input-properties-in-a-binding-expression"></a><span data-ttu-id="3d970-197">Kötési kifejezésekben toocustom bemeneti tulajdonságai más elemekhez köthetők</span><span class="sxs-lookup"><span data-stu-id="3d970-197">Bind toocustom input properties in a binding expression</span></span>

<span data-ttu-id="3d970-198">Kötési kifejezésként is hivatkozhat hello eseményindító hasznos maga definiált tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="3d970-198">Binding expressions can also reference properties that are defined in hello trigger payload itself.</span></span> <span data-ttu-id="3d970-199">Például érdemes lehet toodynamically bind tooa blob storage fájlt egy olyan webhook megadott fájlnév.</span><span class="sxs-lookup"><span data-stu-id="3d970-199">For example, you may want toodynamically bind tooa blob storage file from a filename provided in a webhook.</span></span>

<span data-ttu-id="3d970-200">Például a következő hello *function.json* tulajdonságot használja `BlobName` a hello eseményindító hasznos:</span><span class="sxs-lookup"><span data-stu-id="3d970-200">For example, hello following *function.json* uses a property called `BlobName` from hello trigger payload:</span></span>

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson",
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

<span data-ttu-id="3d970-201">tooaccomplish Ez a C# és F #, meg kell adnia egy POCO, amely meghatározza a deszerializálása hello mezők hello eseményindító tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3d970-201">tooaccomplish this in C# and F#, you must define a POCO that defines hello fields that will be deserialized in hello trigger payload.</span></span>

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

<span data-ttu-id="3d970-202">A JavaScript JSON-deszerializálás automatikusan történik, és közvetlenül hello tulajdonságok használhatók.</span><span class="sxs-lookup"><span data-stu-id="3d970-202">In JavaScript, JSON deserialization is automatically performed and you can use hello properties directly.</span></span>

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a><span data-ttu-id="3d970-203">Futásidőben kötés adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3d970-203">Configuring binding data at runtime</span></span>

<span data-ttu-id="3d970-204">C# és egyéb .NET nyelven, akkor használható egy imperatív kötés mintát deklaratív kötések megakadályozását toohello *function.json*.</span><span class="sxs-lookup"><span data-stu-id="3d970-204">In C# and other .NET languages, you can use an imperative binding pattern, as opposed toohello declarative bindings in *function.json*.</span></span> <span data-ttu-id="3d970-205">Imperatív kötés akkor hasznos, ha a kötési paraméterekhez kell tervezési helyett futásidejű időpontban számított toobe.</span><span class="sxs-lookup"><span data-stu-id="3d970-205">Imperative binding is useful when binding parameters need toobe computed at runtime rather than design time.</span></span> <span data-ttu-id="3d970-206">toolearn több, lásd: [imperatív kötéseken keresztül futásidőben kötés](functions-reference-csharp.md#imperative-bindings) hello C# fejlesztői útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="3d970-206">toolearn more, see [Binding at runtime via imperative bindings](functions-reference-csharp.md#imperative-bindings) in hello C# developer reference.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d970-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d970-207">Next steps</span></span>
<span data-ttu-id="3d970-208">Egy adott kötés további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="3d970-208">For more information on a specific binding, see hello following articles:</span></span>

- [<span data-ttu-id="3d970-209">HTTP és webhookok</span><span class="sxs-lookup"><span data-stu-id="3d970-209">HTTP and webhooks</span></span>](functions-bindings-http-webhook.md)
- [<span data-ttu-id="3d970-210">Időzítő</span><span class="sxs-lookup"><span data-stu-id="3d970-210">Timer</span></span>](functions-bindings-timer.md)
- [<span data-ttu-id="3d970-211">Queue Storage</span><span class="sxs-lookup"><span data-stu-id="3d970-211">Queue storage</span></span>](functions-bindings-storage-queue.md)
- [<span data-ttu-id="3d970-212">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3d970-212">Blob storage</span></span>](functions-bindings-storage-blob.md)
- [<span data-ttu-id="3d970-213">Table Storage</span><span class="sxs-lookup"><span data-stu-id="3d970-213">Table storage</span></span>](functions-bindings-storage-table.md)
- [<span data-ttu-id="3d970-214">Event Hub</span><span class="sxs-lookup"><span data-stu-id="3d970-214">Event Hub</span></span>](functions-bindings-event-hubs.md)
- [<span data-ttu-id="3d970-215">Szolgáltatásbusz</span><span class="sxs-lookup"><span data-stu-id="3d970-215">Service Bus</span></span>](functions-bindings-service-bus.md)
- [<span data-ttu-id="3d970-216">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3d970-216">Cosmos DB</span></span>](functions-bindings-documentdb.md)
- [<span data-ttu-id="3d970-217">SendGrid</span><span class="sxs-lookup"><span data-stu-id="3d970-217">SendGrid</span></span>](functions-bindings-sendgrid.md)
- [<span data-ttu-id="3d970-218">Twilio</span><span class="sxs-lookup"><span data-stu-id="3d970-218">Twilio</span></span>](functions-bindings-twilio.md)
- [<span data-ttu-id="3d970-219">Értesítési központ</span><span class="sxs-lookup"><span data-stu-id="3d970-219">Notification Hubs</span></span>](functions-bindings-notification-hubs.md)
- [<span data-ttu-id="3d970-220">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="3d970-220">Mobile Apps</span></span>](functions-bindings-mobile-apps.md)
- [<span data-ttu-id="3d970-221">Külső fájl</span><span class="sxs-lookup"><span data-stu-id="3d970-221">External file</span></span>](functions-bindings-external-file.md)
