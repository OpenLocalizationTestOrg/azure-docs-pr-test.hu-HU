---
title: "aaaAzure funkciók F # fejlesztői útmutató |} Microsoft Docs"
description: "Megértéséhez hogyan toodevelop Azure Functions használatával F #."
services: functions
documentationcenter: fsharp
author: sylvanc
manager: jbronsk
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, webhookokkal, a dinamikus számítási, a kiszolgáló nélküli architektúra, F #"
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.service: functions
ms.devlang: fsharp
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/09/2016
ms.author: syclebsc
ms.openlocfilehash: 1ac366ba6f73d191c582dcd9214b688ef719617a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-f-developer-reference"></a>Az Azure Functions F # fejlesztői leírás
> [!div class="op_single_selector"]
> * [C# parancsfájl](functions-reference-csharp.md)
> * [F # parancsfájl](functions-reference-fsharp.md)
> * [Node.js](functions-reference-node.md)
> 
> 

F # az Azure Functions egy megoldással egyszerűen futtathatók kisebb kódrészletek, más néven "függvények" hello felhőben. Az F # függvénynek keresztül Függvényargumentumok adatáramlás. Argumentum neve meg van határozva a `function.json`, és nincsenek előre definiált nevek mint függvény naplózó és a megszakítási jogkivonatok hello eléréséhez.

Ez a cikk feltételezi, hogy Ön már elolvasta hello [Azure Functions fejlesztői segédanyagai](functions-reference.md).

## <a name="how-fsx-works"></a>.Fsx működése
Egy `.fsx` fájl az F #-parancsfájlok. Akkor tekinthető az F # projekt egyetlen fájlban található. hello fájl tartalmaz mindkét hello kódját (ebben az esetben, az Azure-függvény) a program és irányelveket függőségek kezelése.

Ha egy `.fsx` egy Azure-függvény, általában a szükséges szerelvényeket automatikusan érhetők el, hogy lehetővé teszi a toofocus hello "bolierplate" helyett függvény kódja.

## <a name="binding-tooarguments"></a>Kötési tooarguments
Minden egyes kötés támogatja-e bizonyos argumentumok, beállítása a részletes hello [Azure Functions eseményindítók és kötések fejlesztői segédanyagai](functions-triggers-bindings.md). Például hello argumentum kötések blob eseményindító támogatja az egyik egy POCO, amelyek használatával az F # rekord jelöl. Példa:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Az F # Azure-függvény lépnek egy vagy több argumentum. Amikor döntésről bővebben az Azure Functions argumentumok, irányítjuk túl*bemeneti* argumentumok és *kimeneti* argumentumok. Egy bemeneti argumentum pontosan mit úgy tűnik, hogy például: bemeneti tooyour F # Azure-függvény. Egy *kimeneti* argumentum változtatható adatok vagy egy `byref<>` vissza módon toopass adatokat ellátó argumentum *kimenő* a függvény.

A fenti, hello példa `blob` egy bemeneti argumentum, és `output` kimeneti argumentum van. Figyelje meg, hogy használtuk `byref<>` a `output` (nincs szükség tooadd hello van `[<Out>]` jegyzet). Használja a `byref<>` típus lehetővé teszi, hogy a függvény toochange, mely rekord vagy objektum hello argumentum hivatkozik.

Az F # rekord mint egy bemeneti típus használatakor hello rekord definition fel kell tüntetni az `[<CLIMutable>]` a rendezés tooallow hello Azure Functions keretrendszer tooset hello mezők megfelelően átadása előtt hello rekord tooyour függvény. A hello technikai `[<CLIMutable>]` hello rekord tulajdonságai a Setter elemek állít elő. Példa:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Az F # osztály mindkét és argumentumokat is használható. Egy osztály tulajdonságok általában kell beolvasókat és Setter elemek. Példa:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Naplózás
a kimeneti tooyour toolog [a folyamatos átviteli naplók](../app-service-web/web-sites-streaming-logs-and-console.md) F #, a függvény típusú argumentumot kell venniük `TraceWriter`. A konzisztencia, javasoljuk, ez az argumentum neve `log`. Példa:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Aszinkron
Hello `async` munkafolyamat is használható, de hello eredmény kell tooreturn egy `Task`. Ezt megteheti a `Async.StartAsTask`, például:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Token törlése
Ha a függvény szabályosan kell toohandle leállítás, amelynek megadhatja egy [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentum. Ez kombinálva `async`, például:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Névterek importálása
Névterek megnyitható hello a szokásos módon:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

a következő névterek hello automatikusan megnyitása:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Külső szerelvények hivatkozik
Hasonlóképpen, a keretrendszer szerelvény hivatkozások adhatók hozzá, amelyeknél hello `#r "AssemblyName"` direktívát.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

hello következő szerelvények automatikusan hello az Azure Functions Gazdakörnyezet:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Ezenkívül a következő szerelvények hello különleges cased és simplename is hivatkozhat (pl. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Belső szerelvény tooreference van szüksége, ha feltöltheti hello szerelvényfájl be egy `bin` mappa relatív tooyour függvény és a hivatkozás (pl. hello azt a fájlnevet  `#r "MyAssembly.dll"`). Hogyan tooupload fájlmappa tooyour függvény a további információkért lásd: a következő szakasz a felügyeleti csomag hello.

## <a name="editor-prelude"></a>Szerkesztő Prelude
Egy szerkesztővel, amely támogatja az F # fordítóprogram szolgáltatások semmit sem éreznek hello névterek és-szerelvényt, amely automatikusan felveszi az Azure Functions. Ilyen lehet hasznos tooinclude egy prelude hello szerelvények használata található hello szerkesztő segítségével, és tooexplicitly nyissa meg a névterek. Példa:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

A kód az Azure Functions végrehajtásakor hello adatforrással, amelynek feldolgozza `COMPILED` definiálva, ezért hello szerkesztő prelude figyelmen kívül hagyja.

<a name="package"></a>

## <a name="package-management"></a>Felügyeleti csomag
az F # függvényben, toouse NuGet-csomagok hozzáadása egy `project.json` toohello hello függvény mappa fájl hello függvény app fájlrendszerben. Példa `project.json` fájl, amely túl ad hozzá a NuGet csomag referenciája`Microsoft.ProjectOxford.Face` 1.1.0-ás verzió:

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

Csak hello .NET-keretrendszer 4.6 támogatott, ezért győződjön meg arról, hogy a `project.json` fájl határozza meg `net46` itt látható módon.

Feltöltése egy `project.json` fájlt, hello futásidejű hello csomagok lekérdezi és automatikusan hozzáadja a hivatkozások toohello csomag szerelvényeket. Nem kell tooadd `#r "AssemblyName"` irányelvek. Csak adja hozzá a szükséges hello `open` utasítások tooyour `.fsx` fájlt.

Kezdésként tooput automatikusan hivatkozik a szerkesztő prelude tooimprove szerelvényeket a szerkesztő interakcióba F # fordítási szolgáltatások.

### <a name="how-tooadd-a-projectjson-file-tooyour-azure-function"></a>Hogyan tooadd egy `project.json` tooyour Azure-függvény fájl
1. Először meggyőződött arról, hogy a függvény alkalmazás fut, amely el ellenőrzéséhez nyissa meg a függvény hello Azure-portálon. Ez is hozzáférést toohello folyamatos átviteli naplók ahol csomag telepítési kimenet jelenik meg.
2. tooupload egy `project.json` fájlt, az ismertetett hello módszerekkel [hogyan tooupdate működni az alkalmazás fájljai](functions-reference.md#fileupdate). Használata [folyamatos üzembe helyezés az Azure Functions](functions-continuous-deployment.md), hozzáadhat egy `project.json` rendelés tooexperiment vele, mielőtt hozzáadná tooyour telepítési fiókirodai ágát átmeneti tooyour fájlt.
3. Hello után `project.json` fájl kerül, látni fogja, például a függvényben a következő kimeneti hasonló toohello tartozó streaming napló:

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
tooget egy környezeti változó vagy egy alkalmazás beállításérték, használjon `System.Environment.GetEnvironmentVariable`, például:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Újbóli felhasználása .fsx kódot
A többi kódot használhatja `.fsx` a fájlok egy `#load` direktívát. Példa:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Elérési utak biztosít toohello `#load` irányelv relatív toohello helyét a `.fsx` fájlt.

* `#load "logger.fsx"`hello függvény mappában található fájl betöltése.
* `#load "package\logger.fsx"`hello található fájl betöltése `package` hello függvény mappája.
* `#load "..\shared\mylogger.fsx"`hello található fájl betöltése `shared` mappát a hello azonos szinten hello függvény mappaként Ez azt jelenti, hogy közvetlenül a `wwwroot`.

Hello `#load` direktíva csak együttműködve `.fsx` (F # parancsfájl) fájlok, és nem a `.fs` fájlokat.

## <a name="next-steps"></a>Következő lépések
További információkért tekintse meg a következő erőforrások hello:

* [F # útmutató](/dotnet/articles/fsharp/index)
* [Azure Functions – ajánlott eljárások](functions-best-practices.md)
* [Az Azure Functions fejlesztői segédanyagai](functions-reference.md)
* [Az Azure Functions C# fejlesztői leírás](functions-reference-csharp.md)
* [Az Azure Functions NodeJS fejlesztői leírás](functions-reference-node.md)
* [Az Azure Functions eseményindítók és kötések](functions-triggers-bindings.md)
* [Az Azure Functions tesztelése](functions-test-a-function.md)
* [Az Azure Functions méretezése](functions-scale.md)

