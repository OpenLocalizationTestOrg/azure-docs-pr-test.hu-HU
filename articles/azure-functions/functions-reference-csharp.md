---
title: "aaaAzure funkciók C# parancsfájl fejlesztői útmutató |} Microsoft Docs"
description: "Megértéséhez hogyan toodevelop Azure Functions használatával C#."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "azure-függvények, függvények, eseményfeldolgozás, webhookok, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: f28cda01-15f3-4047-83f3-e89d5728301c
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/07/2017
ms.author: donnam
ms.openlocfilehash: 27a8f4eb77497a373ff4031539e2e930585e48e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-c-script-developer-reference"></a>Az Azure parancsfájl fejlesztői leírás funkciók C#
> [!div class="op_single_selector"]
> * [C# parancsfájl](functions-reference-csharp.md)
> * [F # parancsfájl](functions-reference-fsharp.md)
> * [Node.js](functions-reference-node.md)
>
>

C# parancsfájl tapasztalattal az Azure Functions hello hello Azure WebJobs SDK alapul. Az adatáramlás a C# függvénynek keresztül metódus argumentumait. Argumentum neve meg van határozva a `function.json`, és nincsenek előre definiált nevek mint függvény naplózó és a megszakítási jogkivonatok hello eléréséhez.

Ez a cikk feltételezi, hogy Ön már elolvasta hello [Azure Functions fejlesztői segédanyagai](functions-reference.md).

Információk az C# osztály szalagtárak című [használó alkalmazás az Azure Functions](functions-dotnet-class-library.md).

## <a name="how-csx-works"></a>.Csx működése
Hello `.csx` formátum lehetővé teszi a toowrite kevesebb mint "bolierplate" és a csak egy C# függvény írása. Például bármely szerelvényhivatkozások és a névterek hello fájl hello elején a szokásos módon. Ahelyett, hogy a névtér és osztály alkalmazásburkoló mindent, csak adja meg a `Run` metódust. Ha tooinclude bármely osztályok, a példány toodefine egyszerű régi CLR-objektum (POCO) objektumok, megadhat egy osztály belsejében hello ugyanazt a fájlt.   

## <a name="binding-tooarguments"></a>Kötési tooarguments
hello különböző kötések kötött tooa C# függvény keresztül hello `name` hello tulajdonság *function.json* konfigurációs. Minden kötésnek rendelkezik saját támogatott típusok; egy blob eseményindító támogathat például egy karakterlánc, egy POCO vagy egy CloudBlockBlob. hello támogatott típusok a kötésben hello útmutatóban szerepelnek. Egy POCO objektumot hozzá elérő és beállító mindegyik tulajdonsághoz megadott kell rendelkeznie.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="using-method-return-value-for-output-binding"></a>A kimeneti kötés metódus visszatérési értéke használatával

Használhatja a metódus visszatérési érték egy kimeneti kötése hello nevének megadásával `$return` a *function.json*:

```json
{
    "type": "queue",
    "direction": "out",
    "name": "$return",
    "queueName": "outqueue",
    "connection": "MyStorageConnectionString",
}
```

```csharp
public static string Run(string input, TraceWriter log)
{
    return input;
}
```

## <a name="writing-multiple-output-values"></a>Több kimeneti értékeinek írása

toowrite több értékek tooan kimeneti kötése, használja a hello [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) vagy [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) típusok. Ezek a típusok csak írható gyűjtemények írásbeli toohello hello metódus befejezésekor a kimeneti kötése.

Ez a példa ír használatával több üzenetek `ICollector`:

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a>Naplózás
toolog kimeneti tooyour folyamatos átviteli naplók C# nyelven íródtak, típusú argumentumot tartalmaz `TraceWriter`. Ajánlott nevezze el `log`. Kerülje a `Console.Write` az Azure Functions. 

`TraceWriter`hello meghatározott [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). a naplózási szint hello `TraceWriter` konfigurálható [állomás\.json].

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Aszinkron
egy aszinkron függvény toomake hello használata `async` kulcsszót, és térjen vissza a `Task` objektum.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a>Token törlése
Egyes műveletek szabályos leállítást igényel. Amíg a rendszer mindig ajánlott toowrite kódot, amelyet kezelni tud összeomló toohandle szabályos leállítást kérelmeket, ahová esetekben adhat a [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) megadott argumentum.  A `CancellationToken` valósul meg, hogy a gazdagép leállítása kiváltásáról toosignal.

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName,
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Névterek importálása
Ha tooimport névterek van szüksége, ehhez a szokásos módon, a hello `using` záradékban.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

hello következő névterek automatikusan importált, ezért nem kötelező:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Külső szerelvények hivatkozik
A keretrendszer szerelvényei közé, hivatkozások hozzáadása hello segítségével `#r "AssemblyName"` direktívát.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

hello következő szerelvények automatikusan hello az Azure Functions Gazdakörnyezet:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

hello következő szerelvényekben lehet hivatkozni egyszerű-név szerint (például `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Egyéni szerelvényeket hivatkozik

egy egyéni szerelvény tooreference, használhatja a *megosztott* szerelvény vagy egy *titkos* szerelvény:
- Közös szerelvényekre összes funkciók, a függvény alkalmazások között vannak megosztva. egy egyéni szerelvény tooreference hello szerelvény tooyour függvény alkalmazás feltöltése, többek között a egy `bin` hello függvény alkalmazás legfelső szintű mappa. 
- Titkos szerelvények egy adott funkció környezet részét képezik, és a tesztcélú különböző verzióit támogatja. Személyes szerelvények fel kell tölteni a egy `bin` hello függvény directory mappájában. Hivatkozás hello fájl nevét, például használatával `#r "MyAssembly.dll"`. 

Hogyan tooupload fájlmappa tooyour függvény a további információkért lásd: a következő szakasz a felügyeleti csomag hello.

### <a name="watched-directories"></a>Figyelt könyvtárak

hello függvény parancsfájlt tartalmazó hello könyvtár automatikusan figyelt a módosítások tooassemblies. a szerelvény változásokat más címtárakban toowatch toohello adja hozzá `watchDirectories` listájában [állomás\.json].

## <a name="using-nuget-packages"></a>NuGet-csomagok használata
NuGet-csomagok toouse C# függvények, töltse fel a *project.json* toohello függvény mappa fájl hello függvény app fájlrendszerben. Példa *project.json* fájl, amely ad hozzá egy hivatkozást tooMicrosoft.ProjectOxford.Face verzió 1.1.0-ás:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Csak hello .NET-keretrendszer 4.6 támogatott, ezért győződjön meg arról, hogy a *project.json* fájl határozza meg `net46` itt látható módon.

Feltöltése egy *project.json* fájlt, hello futásidejű hello csomagok lekérdezi és automatikusan hozzáadja a hivatkozások toohello csomag szerelvényeket. Nem kell tooadd `#r "AssemblyName"` irányelvek. hello NuGet-csomagok definiált toouse hello típusok hozzáadása szükséges hello `using` utasítások tooyour *run.csx* fájl 

Összehasonlításával működik NuGet visszaállítási hello Functions futtatókörnyezete, `project.json` és `project.lock.json`. Ha dátum és idő bélyegzők hello fájlok hello **nem** egyezik, a NuGet-visszaállítás fut, és a NuGet-letöltések csomagok frissítése. Azonban, ha hello dátum és idő bélyegzők hello fájlok **tegye** egyezik, NuGet, a visszaállítás nem hajtható végre. Ezért `project.lock.json` nem kell telepíteni, mivel azt eredményezi, hogy a NuGet tooskip csomag visszaállítási. hello zárolási telepítése tooavoid fájlt, adja hozzá a hello `project.lock.json` toohello `.gitignore` fájlt.

egyéni NuGet-hírcsatorna toouse adja meg az adatcsatorna hello egy *Nuget.Config* hello függvény alkalmazás legfelső szintű fájl. További információkért lásd: [konfigurálása NuGet viselkedés](/nuget/consume-packages/configuring-nuget-behavior).

### <a name="using-a-projectjson-file"></a>Project.json fájl használatával
1. Nyissa meg a hello függvény hello Azure-portálon. hello lapon megjeleníti hello csomag telepítési kimeneti naplózza.
2. egy project.json fájl tooupload hello ismertetett hello módszerek valamelyikével [hogyan tooupdate működni az alkalmazás fájljai](functions-reference.md#fileupdate) hello Azure Functions fejlesztői referencia-témakör a.
3. Hello után *project.json* fájl feltöltése úgy, hogy a következő példa a függvényben hello hasonló kimenetnek meg streaming napló:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Környezeti változók
tooget egy környezeti változó vagy egy alkalmazás beállításérték, használjon `System.Environment.GetEnvironmentVariable`, ahogy az alábbi kódpéldát hello:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " +
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Újbóli felhasználása .csx kódot
Osztályok és az egyéb megadott módszerek *.csx* -fájlok a *run.csx* fájlt. használó, toodo `#load` irányelvek a *run.csx* fájlt. A következő példa hello, a naplózás rutin nevű `MyLogger` a megosztott *myLogger.csx* és betöltve ebbe *run.csx* hello segítségével `#load` irányelv:

Példa *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Példa *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

Egy megosztott használatával *.csx* egy közös mintát, ha azt szeretné, hogy toostrongly a típusargumentumok egy POCO objektummal funkciók között. A következő egyszerűsített példa hello, egy HTTP-eseményindítóval és várólista eseményindító megosztani egy POCO nevű objektum `Order` toostrongly típus hello rendelés adatokat:

Példa *run.csx* HTTP-eseményindító:

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting tooprocessing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Példa *run.csx* várólista eseményindító:

```cs
#load "..\shared\order.csx"

using System;

public static void Run(Order myQueueItem, out Order outputQueueItem,TraceWriter log)
{
    log.Info($"C# Queue trigger function processed order...");
    log.Info(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Példa *order.csx*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +             
                  "\n\tcustAddress : " + custAddress +             
                  "\n\tcustEmail : " + custEmail +             
                  "\n\tcartId : " + cartId + "\n}";             
    }
}
```

Relatív elérési út használatával hello `#load` irányelv:

* `#load "mylogger.csx"`hello függvény mappában található fájl betöltése.
* `#load "loadedfiles\mylogger.csx"`hello függvény mappában található a mappában található fájl betöltése.
* `#load "..\shared\mylogger.csx"`egy mappában hello azonos szinten hello függvény mappaként Ez azt jelenti, hogy a fájlt közvetlenül a *wwwroot*.

Hello `#load` direktíva csak működik *.csx* (C#) parancsfájlok, nem pedig az *.cs* fájlokat.

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a>Imperatív kötéseken keresztül futásidejű kötés

C# és egyéb .NET nyelven, használhat egy [imperatív](https://en.wikipedia.org/wiki/Imperative_programming) kötés mintát, mert megakadályozását toohello [ *deklaratív* ](https://en.wikipedia.org/wiki/Declarative_programming) kötések *function.json*. Imperatív kötés akkor hasznos, ha a kötési paraméterekhez kell tervezési helyett futásidejű időpontban számított toobe. Az ebben a mintában toosupported bemeneti kötése, és kötést a azonnali a kimenetet a funkciókódot.

Adja meg a következő kötés dolgozik:

- **Ne** egy bejegyzést a *function.json* számára a kívánt imperatív kötések.
- A bemeneti paraméter fázis [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) vagy [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- A következő C# mintát tooperform hello adatkötés hello használata.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

Ha `BindingTypeAttribute` hello .NET attribútum, amely meghatározza a kötés és `T` van hello bemeneti vagy kimeneti típus a kötési típus által támogatott. `T`nem lehet egy `out` típusú paraméter (például `out JObject`). Például a Mobile Apps tábla kimeneti kötése támogatja [hat kimeneti típusok](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), de csak használható [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) vagy [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)a `T`.

a következő példakód hello létrehoz egy [tárolási blob kimeneti kötése](functions-bindings-storage-blob.md#using-a-blob-output-binding) blob a futási időben megadott elérési út ezután ír egy karakterlánc toohello blob.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) hello meghatározása [tárolási blob](functions-bindings-storage-blob.md) bemeneti vagy kimeneti kötelező, és [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) támogatott kimeneti kötési típus.
Hello kód kap-e a hello alkalmazás alapértelmezés szerint hello tárolási fiók kapcsolati karakterlánc (amely `AzureWebJobsStorage`). Megadhat egy egyéni alkalmazás-beállítás toouse hozzáadásával a [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) , és átadja hello attribútum tömb be `BindAsync<T>()`. Például:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

hello következő táblázatban hello .NET attribútumok minden kötés típusa és hello csomagok, amelyben vannak definiálva.

> [!div class="mx-codeBreakAll"]
| Kötelező | Attribútum | Hivatkozás hozzáadása |
|------|------|------|
| Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| Storage üzenetsorába | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Storage-blobba | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Tárolási tábla | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a>Következő lépések
További információkért tekintse meg a következő erőforrások hello:

* [Azure Functions – ajánlott eljárások](functions-best-practices.md)
* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)
* [Az Azure Functions F # fejlesztői leírás](functions-reference-fsharp.md)
* [Az Azure Functions NodeJS fejlesztői leírás](functions-reference-node.md)
* [Az Azure Functions eseményindítók és kötések](functions-triggers-bindings.md)

[állomás\.json]: https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json
