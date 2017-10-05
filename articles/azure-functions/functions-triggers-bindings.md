---
title: "Eseményindítók és kötések az Azure Functions együttműködve |} Microsoft Docs"
description: "Megtudhatja, hogyan csatlakozhat eseményindítók és kötések az Azure Functions a kód végrehajtása online események és a felhő alapú szolgáltatások."
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
ms.openlocfilehash: cc41debb2523df77be4db05817a4c7ac55604439
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a><span data-ttu-id="aee53-104">Az Azure Functions eseményindítók és kötések fogalmak</span><span class="sxs-lookup"><span data-stu-id="aee53-104">Azure Functions triggers and bindings concepts</span></span>
<span data-ttu-id="aee53-105">Az Azure Functions lehetővé teszi a kód válaszul Azure-ban és egyéb szolgáltatások események írása keresztül *eseményindítók* és *kötések*.</span><span class="sxs-lookup"><span data-stu-id="aee53-105">Azure Functions allows you to write code in response to events in Azure and other services, through *triggers* and *bindings*.</span></span> <span data-ttu-id="aee53-106">Ez a cikk eseményindítók elméleti áttekintését és kötések az összes támogatott programozási nyelveket.</span><span class="sxs-lookup"><span data-stu-id="aee53-106">This article is a conceptual overview of triggers and bindings for all supported programming languages.</span></span> <span data-ttu-id="aee53-107">Funkciókat, amelyek megegyeznek az összes kötését dokumentum ismerteti.</span><span class="sxs-lookup"><span data-stu-id="aee53-107">Features that are common to all bindings are described here.</span></span>

## <a name="overview"></a><span data-ttu-id="aee53-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="aee53-108">Overview</span></span>

<span data-ttu-id="aee53-109">Eseményindítók és kötések egy deklaratív lehetőséget adja meg, hogyan függvényt hívják, és milyen adatokat is működik.</span><span class="sxs-lookup"><span data-stu-id="aee53-109">Triggers and bindings are a declarative way to define how a function is invoked and what data it works with.</span></span> <span data-ttu-id="aee53-110">A *eseményindító* határozza meg, hogyan függvényt hívják.</span><span class="sxs-lookup"><span data-stu-id="aee53-110">A *trigger* defines how a function is invoked.</span></span> <span data-ttu-id="aee53-111">A függvénynek pontosan egy eseményindító kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="aee53-111">A function must have exactly one trigger.</span></span> <span data-ttu-id="aee53-112">Eseményindítók olyan adatok, amely általában a tartalom, a függvény kiváltó társítva.</span><span class="sxs-lookup"><span data-stu-id="aee53-112">Triggers have associated data, which is usually the payload that triggered the function.</span></span> 

<span data-ttu-id="aee53-113">Bemeneti és kimeneti *kötések* a kód az adatokhoz történő kapcsolódáshoz deklaratív lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="aee53-113">Input and output *bindings* provide a declarative way to connect to data from within your code.</span></span> <span data-ttu-id="aee53-114">Eseményindítók hasonló, megadhatja a kapcsolati karakterláncokat és egyéb tulajdonságok függvény konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="aee53-114">Similar to triggers, you specify connection strings and other properties in your function configuration.</span></span> <span data-ttu-id="aee53-115">Kötések nem kötelező, és egy függvény több bemeneti és a kimeneti kötéseket.</span><span class="sxs-lookup"><span data-stu-id="aee53-115">Bindings are optional and a function can have multiple input and output bindings.</span></span> 

<span data-ttu-id="aee53-116">Az eseményindítók és kötések, írhat kódot a további általános és nem megoldás biztosítja a szolgáltatások részleteit, amely együttműködik.</span><span class="sxs-lookup"><span data-stu-id="aee53-116">Using triggers and bindings, you can write code that is more generic and does not hardcode the details of the services with which it interacts.</span></span> <span data-ttu-id="aee53-117">Egyszerűen vált szolgáltatások bemeneti értékeket a funkciókódot érkező adatokat.</span><span class="sxs-lookup"><span data-stu-id="aee53-117">Data coming from services simply become input values for your function code.</span></span> <span data-ttu-id="aee53-118">A kimeneti adatok (például létrehozhat egy új sor Azure Table Storage-ban) egy másik szolgáltatásba, használja a metódus visszatérési értéke.</span><span class="sxs-lookup"><span data-stu-id="aee53-118">To output data to another service (such as creating a new row in Azure Table Storage), use the return value of the method.</span></span> <span data-ttu-id="aee53-119">Vagy ha több érték kimeneti van szüksége, használja a segítő objektuma.</span><span class="sxs-lookup"><span data-stu-id="aee53-119">Or, if you need to output multiple values, use a helper object.</span></span> <span data-ttu-id="aee53-120">Eseményindítók és kötések rendelkezik egy **neve** tulajdonságot, amelynek azonosítója a kódban a kötés elérésére használhat.</span><span class="sxs-lookup"><span data-stu-id="aee53-120">Triggers and bindings have a **name** property, which is an identifier you use in your code to access the binding.</span></span>

<span data-ttu-id="aee53-121">Eseményindítók és kötések is konfigurálhatja a **integráció** az Azure Functions portálon lapján.</span><span class="sxs-lookup"><span data-stu-id="aee53-121">You can configure triggers and bindings in the **Integrate** tab in the Azure Functions portal.</span></span> <span data-ttu-id="aee53-122">A színfalak a felhasználói felületen módosítja a customdataexample.xml fájlt *function.json* fájl a függvény.</span><span class="sxs-lookup"><span data-stu-id="aee53-122">Under the covers, the UI modifies a file called *function.json* file in the function directory.</span></span> <span data-ttu-id="aee53-123">Ez a fájl szerkesztésével módosítása a **speciális szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="aee53-123">You can edit this file by changing to the **Advanced editor**.</span></span>

<span data-ttu-id="aee53-124">A következő táblázat az eseményindítók és kötések az Azure Functions által támogatott.</span><span class="sxs-lookup"><span data-stu-id="aee53-124">The following table shows the triggers and bindings that are supported with Azure Functions.</span></span> 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a><span data-ttu-id="aee53-125">Példa: a várólista eseményindító és tábla kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="aee53-125">Example: queue trigger and table output binding</span></span>

<span data-ttu-id="aee53-126">Tegyük fel, hogy egy új sort írhat Azure Table Storage, amikor az Azure Queue Storage egy új üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="aee53-126">Suppose you want to write a new row to Azure Table Storage whenever a new message appears in Azure Queue Storage.</span></span> <span data-ttu-id="aee53-127">Ebben a forgatókönyvben az Azure Queue valósítható eseményindító és egy tábla kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="aee53-127">This scenario can be implemented using an Azure Queue trigger and a Table output binding.</span></span> 

<span data-ttu-id="aee53-128">A várólista eseményindító igényel a következő információkat a **integráció** lapon:</span><span class="sxs-lookup"><span data-stu-id="aee53-128">A queue trigger requires the following information in the **Integrate** tab:</span></span>

* <span data-ttu-id="aee53-129">A várólista tárolási fiók kapcsolati karakterláncát tartalmazó az Alkalmazásbeállítás neve</span><span class="sxs-lookup"><span data-stu-id="aee53-129">The name of the app setting that contains the storage account connection string for the queue</span></span>
* <span data-ttu-id="aee53-130">A várólista neve</span><span class="sxs-lookup"><span data-stu-id="aee53-130">The queue name</span></span>
* <span data-ttu-id="aee53-131">A kód beolvassa az üzenetsorban lévő üzenetet tartalmát, mint az azonosító `order`.</span><span class="sxs-lookup"><span data-stu-id="aee53-131">The identifier in your code to read the contents of the queue message, such as `order`.</span></span>

<span data-ttu-id="aee53-132">Az Azure Table Storage használja egy kimeneti kötése a következő adatokkal:</span><span class="sxs-lookup"><span data-stu-id="aee53-132">To write to Azure Table Storage, use an output binding with the following details:</span></span>

* <span data-ttu-id="aee53-133">A tárolási fiók kapcsolati karakterlánc a következő táblázatban található az Alkalmazásbeállítás neve</span><span class="sxs-lookup"><span data-stu-id="aee53-133">The name of the app setting that contains the storage account connection string for the table</span></span>
* <span data-ttu-id="aee53-134">A tábla neve</span><span class="sxs-lookup"><span data-stu-id="aee53-134">The table name</span></span>
* <span data-ttu-id="aee53-135">Az azonosító létrehozása kimeneti elemek vagy az eredményül kapott értéket a függvény a kódban.</span><span class="sxs-lookup"><span data-stu-id="aee53-135">The identifier in your code to create output items, or the return value from the function.</span></span>

<span data-ttu-id="aee53-136">Kötések app beállítások használata a kapcsolati karakterláncok kényszeríteni a legjobb gyakorlat az, hogy *function.json* szolgáltatás titkos kulcsokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="aee53-136">Bindings use app settings for connection strings to enforce the best practice that *function.json* does not contain service secrets.</span></span>

<span data-ttu-id="aee53-137">Ezután használja az Azure Storage integrálása a kódban megadott azonosítók.</span><span class="sxs-lookup"><span data-stu-id="aee53-137">Then, use the identifiers you provided to integrate with Azure Storage in your code.</span></span>

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The method return value creates a new row in Table Storage
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
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
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

<span data-ttu-id="aee53-138">Itt a *function.json* , amely megfelel a fenti kódot.</span><span class="sxs-lookup"><span data-stu-id="aee53-138">Here is the *function.json* that corresponds to the preceding code.</span></span> <span data-ttu-id="aee53-139">Vegye figyelembe, hogy ugyanazt a konfigurációt használhatja, függetlenül attól, a függvény végrehajtása nyelvét.</span><span class="sxs-lookup"><span data-stu-id="aee53-139">Note that the same configuration can be used, regardless of the language of the function implementation.</span></span>

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
<span data-ttu-id="aee53-140">Megtekintheti és szerkesztheti a tartalmát *function.json* az Azure portálon kattintson a **speciális szerkesztő** beállítást a **integráció** lapon, a függvény.</span><span class="sxs-lookup"><span data-stu-id="aee53-140">To view and edit the contents of *function.json* in the Azure portal, click the **Advanced editor** option on the **Integrate** tab of your function.</span></span>

<span data-ttu-id="aee53-141">További példákat és részleteinek integrálása az Azure Storage: [Azure Functions eseményindítók és kötések az Azure Storage](functions-bindings-storage.md).</span><span class="sxs-lookup"><span data-stu-id="aee53-141">For more code examples and details on integrating with Azure Storage, see [Azure Functions triggers and bindings for Azure Storage](functions-bindings-storage.md).</span></span>

### <a name="binding-direction"></a><span data-ttu-id="aee53-142">Kötési iránya</span><span class="sxs-lookup"><span data-stu-id="aee53-142">Binding direction</span></span>

<span data-ttu-id="aee53-143">Az összes eseményindítók és kötések vannak egy `direction` tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="aee53-143">All triggers and bindings have a `direction` property:</span></span>

- <span data-ttu-id="aee53-144">Az eseményindítók az irány mindig van kapcsolva`in`</span><span class="sxs-lookup"><span data-stu-id="aee53-144">For triggers, the direction is always `in`</span></span>
- <span data-ttu-id="aee53-145">Bemeneti és kimeneti kötések használhatják `in` és`out`</span><span class="sxs-lookup"><span data-stu-id="aee53-145">Input and output bindings use `in` and `out`</span></span>
- <span data-ttu-id="aee53-146">Néhány kötések támogatja a speciális paraméterirányt `inout`.</span><span class="sxs-lookup"><span data-stu-id="aee53-146">Some bindings support a special direction `inout`.</span></span> <span data-ttu-id="aee53-147">Ha `inout`, csak a **speciális szerkesztő** érhető el a **integráció** fülre.</span><span class="sxs-lookup"><span data-stu-id="aee53-147">If you use `inout`, only the **Advanced editor** is available in the **Integrate** tab.</span></span>

## <a name="using-the-function-return-type-to-return-a-single-output"></a><span data-ttu-id="aee53-148">A függvény visszatérési típusa használatával egyetlen kimeneti vissza</span><span class="sxs-lookup"><span data-stu-id="aee53-148">Using the function return type to return a single output</span></span>

<span data-ttu-id="aee53-149">Az előző példa bemutatja, hogyan kimeneti egy kötést, amely használatával a különleges name paramétert adja meg a függvény visszatérési értéke használandó `$return`.</span><span class="sxs-lookup"><span data-stu-id="aee53-149">The preceding example shows how to use the function return value to provide output to a binding, which is achieved by using the special name parameter `$return`.</span></span> <span data-ttu-id="aee53-150">(Ez csak akkor támogatott a nyelveket, amelyeken a visszatérési érték, például a C#, JavaScript és F #.) Ha egy függvény több kimeneti kötése, `$return` csak az egyik a kimeneti kötéseket.</span><span class="sxs-lookup"><span data-stu-id="aee53-150">(This is only supported in languages that have a return value, such as C#, JavaScript, and F#.) If a function has multiple output bindings, use `$return` for only one of the output bindings.</span></span> 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

<span data-ttu-id="aee53-151">Az alábbi megjelenítése Példák hogyan visszatérési típusok használhatók kimeneti kötések C#, JavaScript és F #.</span><span class="sxs-lookup"><span data-stu-id="aee53-151">The examples below show how return types are used with output bindings in C#, JavaScript, and F#.</span></span>

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
// JavaScript: return a value in the second parameter to context.done
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

## <a name="binding-datatype-property"></a><span data-ttu-id="aee53-152">Kötés dataType tulajdonsága</span><span class="sxs-lookup"><span data-stu-id="aee53-152">Binding dataType property</span></span>

<span data-ttu-id="aee53-153">A .NET használja a bemeneti adatok adatok típusának azonosítására.</span><span class="sxs-lookup"><span data-stu-id="aee53-153">In .NET, use the types to define the data type for input data.</span></span> <span data-ttu-id="aee53-154">Használja például a `string` kötődni a várólista eseményindítót, valamint egy bájttömböt olvasni bináris formában.</span><span class="sxs-lookup"><span data-stu-id="aee53-154">For instance, use `string` to bind to the text of a queue trigger and a byte array to read as binary.</span></span>

<span data-ttu-id="aee53-155">Például a JavaScriptek dinamikusan beírt nyelven, használja a `dataType` tulajdonság kötése definíciójában.</span><span class="sxs-lookup"><span data-stu-id="aee53-155">For languages that are dynamically typed such as JavaScript, use the `dataType` property in the binding definition.</span></span> <span data-ttu-id="aee53-156">Olvassa el a tartalom HTTP-kérések bináris formátumú, például típust használjon `binary`:</span><span class="sxs-lookup"><span data-stu-id="aee53-156">For example, to read the content of an HTTP request in binary format, use the type `binary`:</span></span>

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

<span data-ttu-id="aee53-157">Más beállításokat a `dataType` vannak `stream` és `string`.</span><span class="sxs-lookup"><span data-stu-id="aee53-157">Other options for `dataType` are `stream` and `string`.</span></span>

## <a name="resolving-app-settings"></a><span data-ttu-id="aee53-158">Alkalmazásbeállítások feloldása</span><span class="sxs-lookup"><span data-stu-id="aee53-158">Resolving app settings</span></span>
<span data-ttu-id="aee53-159">Ajánlott eljárásként titkos kulcsok és a kapcsolati karakterláncok használatával kell irányítani Alkalmazásbeállítások, nem pedig konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="aee53-159">As a best practice, secrets and connection strings should be managed using app settings, rather than configuration files.</span></span> <span data-ttu-id="aee53-160">Ez korlátozza, hogy ezeknek a kulcsoknak access, és lehetővé teszi a biztonságos tárolására *function.json* a egy nyilvános verziókövetési tárházzal.</span><span class="sxs-lookup"><span data-stu-id="aee53-160">This limits access to these secrets and makes it safe to store *function.json* in a public source control repository.</span></span>

<span data-ttu-id="aee53-161">Alkalmazásbeállítások is hasznosak, ha meg szeretné változtatni a konfiguráció a környezet alapján.</span><span class="sxs-lookup"><span data-stu-id="aee53-161">App settings are also useful whenever you want to change configuration based on the environment.</span></span> <span data-ttu-id="aee53-162">Például egy tesztkörnyezetben, érdemes lehet egy másik várólista vagy a blob-tároló figyelésére.</span><span class="sxs-lookup"><span data-stu-id="aee53-162">For example, in a test environment, you may want to monitor a different queue or blob storage container.</span></span>

<span data-ttu-id="aee53-163">Alkalmazásbeállítások fakadó problémák megoldásával, amikor egy érték szimpla százalékjelek, például a `%MyAppSetting%`.</span><span class="sxs-lookup"><span data-stu-id="aee53-163">App settings are resolved whenever a value is enclosed in percent signs, such as `%MyAppSetting%`.</span></span> <span data-ttu-id="aee53-164">Vegye figyelembe, hogy a `connection` eseményindítók és kötések tulajdonsága egy különleges esetben, és automatikusan feloldja az értékeket, ha az alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="aee53-164">Note that the `connection` property of triggers and bindings is a special case and automatically resolves values as app settings.</span></span> 

<span data-ttu-id="aee53-165">A következő példa egy Alkalmazásbeállítás használó várólista eseményindító `%input-queue-name%` elindítani a várólista meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="aee53-165">The following example is a queue trigger that uses an app setting `%input-queue-name%` to define the queue to trigger on.</span></span>

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

## <a name="trigger-metadata-properties"></a><span data-ttu-id="aee53-166">Eseményindító metaadat-tulajdonságainak</span><span class="sxs-lookup"><span data-stu-id="aee53-166">Trigger metadata properties</span></span>

<span data-ttu-id="aee53-167">Sok eseményindítók mellett a hasznos adatforgalmat egy eseményindító (például az üzenetsorban található üzenetet függvény kiváltó) által biztosított, adja meg a további metaadatokat értékét.</span><span class="sxs-lookup"><span data-stu-id="aee53-167">In addition to the data payload provided by a trigger (such as the queue message that triggered a function), many triggers provide additional metadata values.</span></span> <span data-ttu-id="aee53-168">Ezeket az értékeket a C# és F # vagy tulajdonságok bemeneti paraméter használható a `context.bindings` JavaScript objektumban.</span><span class="sxs-lookup"><span data-stu-id="aee53-168">These values can be used as input parameters in C# and F# or properties on the `context.bindings` object in JavaScript.</span></span> 

<span data-ttu-id="aee53-169">Például egy várólista eseményindító támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="aee53-169">For example, a queue trigger supports the following properties:</span></span>

* <span data-ttu-id="aee53-170">QueueTrigger - indítására üzenet tartalmát, ha egy érvényes karakterláncot</span><span class="sxs-lookup"><span data-stu-id="aee53-170">QueueTrigger - triggering message content if a valid string</span></span>
* <span data-ttu-id="aee53-171">DequeueCount</span><span class="sxs-lookup"><span data-stu-id="aee53-171">DequeueCount</span></span>
* <span data-ttu-id="aee53-172">ExpirationTime</span><span class="sxs-lookup"><span data-stu-id="aee53-172">ExpirationTime</span></span>
* <span data-ttu-id="aee53-173">Azonosító</span><span class="sxs-lookup"><span data-stu-id="aee53-173">Id</span></span>
* <span data-ttu-id="aee53-174">InsertionTime</span><span class="sxs-lookup"><span data-stu-id="aee53-174">InsertionTime</span></span>
* <span data-ttu-id="aee53-175">NextVisibleTime</span><span class="sxs-lookup"><span data-stu-id="aee53-175">NextVisibleTime</span></span>
* <span data-ttu-id="aee53-176">PopReceipt</span><span class="sxs-lookup"><span data-stu-id="aee53-176">PopReceipt</span></span>

<span data-ttu-id="aee53-177">A megfelelő referencia-témakör ismerteti a metaadat-tulajdonságainak minden eseményindító részleteit.</span><span class="sxs-lookup"><span data-stu-id="aee53-177">Details of metadata properties for each trigger are described in the corresponding reference topic.</span></span> <span data-ttu-id="aee53-178">Dokumentáció is rendelkezésre áll, az a **integráció** a portál lapján, a a **dokumentáció** című szakaszt a kötési konfigurációja területen.</span><span class="sxs-lookup"><span data-stu-id="aee53-178">Documentation is also available in the **Integrate** tab of the portal, in the **Documentation** section below the binding configuration area.</span></span>  

<span data-ttu-id="aee53-179">Például blob eseményindítók rendelkezik néhány késések, mivel segítségével várólista eseményindító futtassa a funkciót (lásd: [Blob Storage eseményindító](functions-bindings-storage-blob.md#storage-blob-trigger).</span><span class="sxs-lookup"><span data-stu-id="aee53-179">For example, since blob triggers have some delays, you can use a queue trigger to run your function (see [Blob Storage Trigger](functions-bindings-storage-blob.md#storage-blob-trigger).</span></span> <span data-ttu-id="aee53-180">Az üzenetsorban lévő üzenetet tartalmaz a blob fájlnevét elindítani a.</span><span class="sxs-lookup"><span data-stu-id="aee53-180">The queue message would contain the blob filename to trigger on.</span></span> <span data-ttu-id="aee53-181">Használja a `queueTrigger` metaadat-tulajdonságnak adhat meg ezt a viselkedést összes konfigurációjáról, nem pedig a kódot.</span><span class="sxs-lookup"><span data-stu-id="aee53-181">Using the `queueTrigger` metadata property, you can specify this behavior all in your configuration, rather than your code.</span></span>

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

<span data-ttu-id="aee53-182">Egy metaadat-tulajdonságot is használható egy *kötési kifejezés* egy másik kötés, a következő szakaszban leírt módon.</span><span class="sxs-lookup"><span data-stu-id="aee53-182">Metadata properties from a trigger can also be used in a *binding expression* for another binding, as described in the following section.</span></span>

## <a name="binding-expressions-and-patterns"></a><span data-ttu-id="aee53-183">Kötelező kifejezések és minták</span><span class="sxs-lookup"><span data-stu-id="aee53-183">Binding expressions and patterns</span></span>

<span data-ttu-id="aee53-184">Az egyik leghatékonyabb részeit, eseményindítók és kötések *kötési kifejezésként*.</span><span class="sxs-lookup"><span data-stu-id="aee53-184">One of the most powerful features of triggers and bindings is *binding expressions*.</span></span> <span data-ttu-id="aee53-185">A kötés belül mintát kifejezések, amelyek ezután felhasználhatók adhat meg más kötésekben vagy a kód.</span><span class="sxs-lookup"><span data-stu-id="aee53-185">Within your binding, you can define pattern expressions which can then be used in other bindings or your code.</span></span> <span data-ttu-id="aee53-186">Eseményindító metaadatait is használható a kötési kifejezésként, mint az előző szakaszban leírt minta megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="aee53-186">Trigger metadata can also be used in binding expressions, as show in the sample in the preceding section.</span></span>

<span data-ttu-id="aee53-187">Tegyük fel például, adott blob storage tárolóban, hasonló képek átméretezése szeretné a **kép méret** -sablon a **új függvény** lap.</span><span class="sxs-lookup"><span data-stu-id="aee53-187">For example, suppose you want to resize images in particular blob storage container, similar to the **Image Resizer** template in the **New Function** page.</span></span> <span data-ttu-id="aee53-188">Nyissa meg a **új függvény** -> nyelvi **C#** forgatókönyv -> **minták** -> **ImageResizer-c Sharp**.</span><span class="sxs-lookup"><span data-stu-id="aee53-188">Go to **New Function** -> Language **C#** -> Scenario **Samples** -> **ImageResizer-CSharp**.</span></span> 

<span data-ttu-id="aee53-189">Itt a *function.json* definíciója:</span><span class="sxs-lookup"><span data-stu-id="aee53-189">Here is the *function.json* definition:</span></span>

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

<span data-ttu-id="aee53-190">Figyelje meg, hogy a `filename` paraméter van megadva a mind a blob eseményindító definícióját, valamint a blob kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="aee53-190">Notice that the `filename` parameter is used in both the blob trigger definition as well as the blob output binding.</span></span> <span data-ttu-id="aee53-191">Ez a paraméter funkciókódot is használható.</span><span class="sxs-lookup"><span data-stu-id="aee53-191">This parameter can also be used in function code.</span></span>

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a><span data-ttu-id="aee53-192">Véletlenszerű GUID azonosítók</span><span class="sxs-lookup"><span data-stu-id="aee53-192">Random GUIDs</span></span>
<span data-ttu-id="aee53-193">Az Azure Functions kényelmi szintaxist tartalmaz a GUID előállítása érdekében a kötéseiben keresztül a `{rand-guid}` kötési kifejezés.</span><span class="sxs-lookup"><span data-stu-id="aee53-193">Azure Functions provides a convenience syntax for generating GUIDs in your bindings, through the `{rand-guid}` binding expression.</span></span> <span data-ttu-id="aee53-194">Az alábbi példa használja ahhoz, hogy a blob egyedi nevet létrehozni:</span><span class="sxs-lookup"><span data-stu-id="aee53-194">The following example uses this to generate a unique blob name:</span></span> 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a><span data-ttu-id="aee53-195">Aktuális idő</span><span class="sxs-lookup"><span data-stu-id="aee53-195">Current time</span></span>

<span data-ttu-id="aee53-196">A kötési kifejezés használható `DateTime`, amely feloldása egy olyan `DateTime.UtcNow`.</span><span class="sxs-lookup"><span data-stu-id="aee53-196">You can use the binding expression `DateTime`, which resolves to `DateTime.UtcNow`.</span></span>

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties-in-a-binding-expression"></a><span data-ttu-id="aee53-197">A kötési kifejezése egyéni bemeneti tulajdonságok kötése</span><span class="sxs-lookup"><span data-stu-id="aee53-197">Bind to custom input properties in a binding expression</span></span>

<span data-ttu-id="aee53-198">Kötési kifejezésként tulajdonságok határozzák meg az eseményindító forgalma maga is hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="aee53-198">Binding expressions can also reference properties that are defined in the trigger payload itself.</span></span> <span data-ttu-id="aee53-199">Érdemes lehet például egy olyan webhook megadott fájlnév a blob storage fájlba dinamikusan kötni.</span><span class="sxs-lookup"><span data-stu-id="aee53-199">For example, you may want to dynamically bind to a blob storage file from a filename provided in a webhook.</span></span>

<span data-ttu-id="aee53-200">Például a következő *function.json* tulajdonságot használja `BlobName` az eseményindító forgalma a:</span><span class="sxs-lookup"><span data-stu-id="aee53-200">For example, the following *function.json* uses a property called `BlobName` from the trigger payload:</span></span>

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

<span data-ttu-id="aee53-201">Ehhez a C# és F #, meg kell határoznia egy POCO, amely meghatározza a mezőket, amelyeknek deszerializálása az eseményindító tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="aee53-201">To accomplish this in C# and F#, you must define a POCO that defines the fields that will be deserialized in the trigger payload.</span></span>

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

<span data-ttu-id="aee53-202">A JavaScript JSON-deszerializálás automatikusan történik, és közvetlenül a tulajdonságok használhatók.</span><span class="sxs-lookup"><span data-stu-id="aee53-202">In JavaScript, JSON deserialization is automatically performed and you can use the properties directly.</span></span>

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

## <a name="configuring-binding-data-at-runtime"></a><span data-ttu-id="aee53-203">Futásidőben kötés adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aee53-203">Configuring binding data at runtime</span></span>

<span data-ttu-id="aee53-204">C# és egyéb .NET-nyelveket, használhat egy imperatív kötés mintát, szemben a deklaratív kötések *function.json*.</span><span class="sxs-lookup"><span data-stu-id="aee53-204">In C# and other .NET languages, you can use an imperative binding pattern, as opposed to the declarative bindings in *function.json*.</span></span> <span data-ttu-id="aee53-205">Imperatív kötés akkor hasznos, ha a kötési paraméterekhez kell számítani a Tervező helyett futásidejű időpontban.</span><span class="sxs-lookup"><span data-stu-id="aee53-205">Imperative binding is useful when binding parameters need to be computed at runtime rather than design time.</span></span> <span data-ttu-id="aee53-206">További tudnivalókért lásd: [imperatív kötéseken keresztül futásidőben kötés](functions-reference-csharp.md#imperative-bindings) a C# fejlesztői útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="aee53-206">To learn more, see [Binding at runtime via imperative bindings](functions-reference-csharp.md#imperative-bindings) in the C# developer reference.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aee53-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aee53-207">Next steps</span></span>
<span data-ttu-id="aee53-208">Egy adott kötés további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="aee53-208">For more information on a specific binding, see the following articles:</span></span>

- [<span data-ttu-id="aee53-209">HTTP és webhookok</span><span class="sxs-lookup"><span data-stu-id="aee53-209">HTTP and webhooks</span></span>](functions-bindings-http-webhook.md)
- [<span data-ttu-id="aee53-210">Időzítő</span><span class="sxs-lookup"><span data-stu-id="aee53-210">Timer</span></span>](functions-bindings-timer.md)
- [<span data-ttu-id="aee53-211">Queue Storage</span><span class="sxs-lookup"><span data-stu-id="aee53-211">Queue storage</span></span>](functions-bindings-storage-queue.md)
- [<span data-ttu-id="aee53-212">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="aee53-212">Blob storage</span></span>](functions-bindings-storage-blob.md)
- [<span data-ttu-id="aee53-213">Table Storage</span><span class="sxs-lookup"><span data-stu-id="aee53-213">Table storage</span></span>](functions-bindings-storage-table.md)
- [<span data-ttu-id="aee53-214">Event Hub</span><span class="sxs-lookup"><span data-stu-id="aee53-214">Event Hub</span></span>](functions-bindings-event-hubs.md)
- [<span data-ttu-id="aee53-215">Szolgáltatásbusz</span><span class="sxs-lookup"><span data-stu-id="aee53-215">Service Bus</span></span>](functions-bindings-service-bus.md)
- [<span data-ttu-id="aee53-216">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="aee53-216">Cosmos DB</span></span>](functions-bindings-documentdb.md)
- [<span data-ttu-id="aee53-217">SendGrid</span><span class="sxs-lookup"><span data-stu-id="aee53-217">SendGrid</span></span>](functions-bindings-sendgrid.md)
- [<span data-ttu-id="aee53-218">Twilio</span><span class="sxs-lookup"><span data-stu-id="aee53-218">Twilio</span></span>](functions-bindings-twilio.md)
- [<span data-ttu-id="aee53-219">Értesítési központ</span><span class="sxs-lookup"><span data-stu-id="aee53-219">Notification Hubs</span></span>](functions-bindings-notification-hubs.md)
- [<span data-ttu-id="aee53-220">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="aee53-220">Mobile Apps</span></span>](functions-bindings-mobile-apps.md)
- [<span data-ttu-id="aee53-221">Külső fájl</span><span class="sxs-lookup"><span data-stu-id="aee53-221">External file</span></span>](functions-bindings-external-file.md)
