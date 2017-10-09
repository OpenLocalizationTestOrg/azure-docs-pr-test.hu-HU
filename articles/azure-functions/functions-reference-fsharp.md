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
# <a name="azure-functions-f-developer-reference"></a><span data-ttu-id="18277-104">Az Azure Functions F # fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="18277-104">Azure Functions F# Developer Reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="18277-105">C# parancsfájl</span><span class="sxs-lookup"><span data-stu-id="18277-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="18277-106">F # parancsfájl</span><span class="sxs-lookup"><span data-stu-id="18277-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="18277-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="18277-107">Node.js</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="18277-108">F # az Azure Functions egy megoldással egyszerűen futtathatók kisebb kódrészletek, más néven "függvények" hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="18277-108">F# for Azure Functions is a solution for easily running small pieces of code, or "functions," in hello cloud.</span></span> <span data-ttu-id="18277-109">Az F # függvénynek keresztül Függvényargumentumok adatáramlás.</span><span class="sxs-lookup"><span data-stu-id="18277-109">Data flows into your F# function via function arguments.</span></span> <span data-ttu-id="18277-110">Argumentum neve meg van határozva a `function.json`, és nincsenek előre definiált nevek mint függvény naplózó és a megszakítási jogkivonatok hello eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="18277-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like hello function logger and cancellation tokens.</span></span>

<span data-ttu-id="18277-111">Ez a cikk feltételezi, hogy Ön már elolvasta hello [Azure Functions fejlesztői segédanyagai](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="18277-111">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="how-fsx-works"></a><span data-ttu-id="18277-112">.Fsx működése</span><span class="sxs-lookup"><span data-stu-id="18277-112">How .fsx works</span></span>
<span data-ttu-id="18277-113">Egy `.fsx` fájl az F #-parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="18277-113">An `.fsx` file is an F# script.</span></span> <span data-ttu-id="18277-114">Akkor tekinthető az F # projekt egyetlen fájlban található.</span><span class="sxs-lookup"><span data-stu-id="18277-114">It can be thought of as an F# project that's contained in a single file.</span></span> <span data-ttu-id="18277-115">hello fájl tartalmaz mindkét hello kódját (ebben az esetben, az Azure-függvény) a program és irányelveket függőségek kezelése.</span><span class="sxs-lookup"><span data-stu-id="18277-115">hello file contains both hello code for your program (in this case, your Azure Function) and directives for managing dependencies.</span></span>

<span data-ttu-id="18277-116">Ha egy `.fsx` egy Azure-függvény, általában a szükséges szerelvényeket automatikusan érhetők el, hogy lehetővé teszi a toofocus hello "bolierplate" helyett függvény kódja.</span><span class="sxs-lookup"><span data-stu-id="18277-116">When you use an `.fsx` for an Azure Function, commonly required assemblies are automatically included for you, allowing you toofocus on hello function rather than "boilerplate" code.</span></span>

## <a name="binding-tooarguments"></a><span data-ttu-id="18277-117">Kötési tooarguments</span><span class="sxs-lookup"><span data-stu-id="18277-117">Binding tooarguments</span></span>
<span data-ttu-id="18277-118">Minden egyes kötés támogatja-e bizonyos argumentumok, beállítása a részletes hello [Azure Functions eseményindítók és kötések fejlesztői segédanyagai](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="18277-118">Each binding supports some set of arguments, as detailed in hello [Azure Functions triggers and bindings developer reference](functions-triggers-bindings.md).</span></span> <span data-ttu-id="18277-119">Például hello argumentum kötések blob eseményindító támogatja az egyik egy POCO, amelyek használatával az F # rekord jelöl.</span><span class="sxs-lookup"><span data-stu-id="18277-119">For example, one of hello argument bindings a blob trigger supports is a POCO, which can be expressed using an F# record.</span></span> <span data-ttu-id="18277-120">Példa:</span><span class="sxs-lookup"><span data-stu-id="18277-120">For example:</span></span>

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

<span data-ttu-id="18277-121">Az F # Azure-függvény lépnek egy vagy több argumentum.</span><span class="sxs-lookup"><span data-stu-id="18277-121">Your F# Azure Function will take one or more arguments.</span></span> <span data-ttu-id="18277-122">Amikor döntésről bővebben az Azure Functions argumentumok, irányítjuk túl*bemeneti* argumentumok és *kimeneti* argumentumok.</span><span class="sxs-lookup"><span data-stu-id="18277-122">When we talk about Azure Functions arguments, we refer too*input* arguments and *output* arguments.</span></span> <span data-ttu-id="18277-123">Egy bemeneti argumentum pontosan mit úgy tűnik, hogy például: bemeneti tooyour F # Azure-függvény.</span><span class="sxs-lookup"><span data-stu-id="18277-123">An input argument is exactly what it sounds like: input tooyour F# Azure Function.</span></span> <span data-ttu-id="18277-124">Egy *kimeneti* argumentum változtatható adatok vagy egy `byref<>` vissza módon toopass adatokat ellátó argumentum *kimenő* a függvény.</span><span class="sxs-lookup"><span data-stu-id="18277-124">An *output* argument is mutable data or a `byref<>` argument that serves as a way toopass data back *out* of your function.</span></span>

<span data-ttu-id="18277-125">A fenti, hello példa `blob` egy bemeneti argumentum, és `output` kimeneti argumentum van.</span><span class="sxs-lookup"><span data-stu-id="18277-125">In hello example above, `blob` is an input argument, and `output` is an output argument.</span></span> <span data-ttu-id="18277-126">Figyelje meg, hogy használtuk `byref<>` a `output` (nincs szükség tooadd hello van `[<Out>]` jegyzet).</span><span class="sxs-lookup"><span data-stu-id="18277-126">Notice that we used `byref<>` for `output` (there's no need tooadd hello `[<Out>]` annotation).</span></span> <span data-ttu-id="18277-127">Használja a `byref<>` típus lehetővé teszi, hogy a függvény toochange, mely rekord vagy objektum hello argumentum hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="18277-127">Using a `byref<>` type allows your function toochange which record or object hello argument refers to.</span></span>

<span data-ttu-id="18277-128">Az F # rekord mint egy bemeneti típus használatakor hello rekord definition fel kell tüntetni az `[<CLIMutable>]` a rendezés tooallow hello Azure Functions keretrendszer tooset hello mezők megfelelően átadása előtt hello rekord tooyour függvény.</span><span class="sxs-lookup"><span data-stu-id="18277-128">When an F# record is used as an input type, hello record definition must be marked with `[<CLIMutable>]` in order tooallow hello Azure Functions framework tooset hello fields appropriately before passing hello record tooyour function.</span></span> <span data-ttu-id="18277-129">A hello technikai `[<CLIMutable>]` hello rekord tulajdonságai a Setter elemek állít elő.</span><span class="sxs-lookup"><span data-stu-id="18277-129">Under hello hood, `[<CLIMutable>]` generates setters for hello record properties.</span></span> <span data-ttu-id="18277-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="18277-130">For example:</span></span>

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

<span data-ttu-id="18277-131">Az F # osztály mindkét és argumentumokat is használható.</span><span class="sxs-lookup"><span data-stu-id="18277-131">An F# class can also be used for both in and out arguments.</span></span> <span data-ttu-id="18277-132">Egy osztály tulajdonságok általában kell beolvasókat és Setter elemek.</span><span class="sxs-lookup"><span data-stu-id="18277-132">For a class, properties will usually need getters and setters.</span></span> <span data-ttu-id="18277-133">Példa:</span><span class="sxs-lookup"><span data-stu-id="18277-133">For example:</span></span>

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a><span data-ttu-id="18277-134">Naplózás</span><span class="sxs-lookup"><span data-stu-id="18277-134">Logging</span></span>
<span data-ttu-id="18277-135">a kimeneti tooyour toolog [a folyamatos átviteli naplók](../app-service-web/web-sites-streaming-logs-and-console.md) F #, a függvény típusú argumentumot kell venniük `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="18277-135">toolog output tooyour [streaming logs](../app-service-web/web-sites-streaming-logs-and-console.md) in F#, your function should take an argument of type `TraceWriter`.</span></span> <span data-ttu-id="18277-136">A konzisztencia, javasoljuk, ez az argumentum neve `log`.</span><span class="sxs-lookup"><span data-stu-id="18277-136">For consistency, we recommend this argument is named `log`.</span></span> <span data-ttu-id="18277-137">Példa:</span><span class="sxs-lookup"><span data-stu-id="18277-137">For example:</span></span>

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a><span data-ttu-id="18277-138">Aszinkron</span><span class="sxs-lookup"><span data-stu-id="18277-138">Async</span></span>
<span data-ttu-id="18277-139">Hello `async` munkafolyamat is használható, de hello eredmény kell tooreturn egy `Task`.</span><span class="sxs-lookup"><span data-stu-id="18277-139">hello `async` workflow can be used, but hello result needs tooreturn a `Task`.</span></span> <span data-ttu-id="18277-140">Ezt megteheti a `Async.StartAsTask`, például:</span><span class="sxs-lookup"><span data-stu-id="18277-140">This can be done with `Async.StartAsTask`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a><span data-ttu-id="18277-141">Token törlése</span><span class="sxs-lookup"><span data-stu-id="18277-141">Cancellation Token</span></span>
<span data-ttu-id="18277-142">Ha a függvény szabályosan kell toohandle leállítás, amelynek megadhatja egy [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentum.</span><span class="sxs-lookup"><span data-stu-id="18277-142">If your function needs toohandle shutdown gracefully, you can give it a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument.</span></span> <span data-ttu-id="18277-143">Ez kombinálva `async`, például:</span><span class="sxs-lookup"><span data-stu-id="18277-143">This can be combined with `async`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a><span data-ttu-id="18277-144">Névterek importálása</span><span class="sxs-lookup"><span data-stu-id="18277-144">Importing namespaces</span></span>
<span data-ttu-id="18277-145">Névterek megnyitható hello a szokásos módon:</span><span class="sxs-lookup"><span data-stu-id="18277-145">Namespaces can be opened in hello usual way:</span></span>

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="18277-146">a következő névterek hello automatikusan megnyitása:</span><span class="sxs-lookup"><span data-stu-id="18277-146">hello following namespaces are automatically opened:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* <span data-ttu-id="18277-147">`Microsoft.Azure.WebJobs.Host`.</span><span class="sxs-lookup"><span data-stu-id="18277-147">`Microsoft.Azure.WebJobs.Host`.</span></span>

## <a name="referencing-external-assemblies"></a><span data-ttu-id="18277-148">Külső szerelvények hivatkozik</span><span class="sxs-lookup"><span data-stu-id="18277-148">Referencing External Assemblies</span></span>
<span data-ttu-id="18277-149">Hasonlóképpen, a keretrendszer szerelvény hivatkozások adhatók hozzá, amelyeknél hello `#r "AssemblyName"` direktívát.</span><span class="sxs-lookup"><span data-stu-id="18277-149">Similarly, framework assembly references be added with hello `#r "AssemblyName"` directive.</span></span>

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="18277-150">hello következő szerelvények automatikusan hello az Azure Functions Gazdakörnyezet:</span><span class="sxs-lookup"><span data-stu-id="18277-150">hello following assemblies are automatically added by hello Azure Functions hosting environment:</span></span>

* <span data-ttu-id="18277-151">`mscorlib`,</span><span class="sxs-lookup"><span data-stu-id="18277-151">`mscorlib`,</span></span>
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* <span data-ttu-id="18277-152">`System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="18277-152">`System.Net.Http.Formatting`.</span></span>

<span data-ttu-id="18277-153">Ezenkívül a következő szerelvények hello különleges cased és simplename is hivatkozhat (pl. `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="18277-153">In addition, hello following assemblies are special cased and may be referenced by simplename (e.g. `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* <span data-ttu-id="18277-154">`Microsoft.AspNEt.WebHooks.Common`.</span><span class="sxs-lookup"><span data-stu-id="18277-154">`Microsoft.AspNEt.WebHooks.Common`.</span></span>

<span data-ttu-id="18277-155">Belső szerelvény tooreference van szüksége, ha feltöltheti hello szerelvényfájl be egy `bin` mappa relatív tooyour függvény és a hivatkozás (pl. hello azt a fájlnevet  `#r "MyAssembly.dll"`).</span><span class="sxs-lookup"><span data-stu-id="18277-155">If you need tooreference a private assembly, you can upload hello assembly file into a `bin` folder relative tooyour function and reference it by using hello file name (e.g.  `#r "MyAssembly.dll"`).</span></span> <span data-ttu-id="18277-156">Hogyan tooupload fájlmappa tooyour függvény a további információkért lásd: a következő szakasz a felügyeleti csomag hello.</span><span class="sxs-lookup"><span data-stu-id="18277-156">For information on how tooupload files tooyour function folder, see hello following section on package management.</span></span>

## <a name="editor-prelude"></a><span data-ttu-id="18277-157">Szerkesztő Prelude</span><span class="sxs-lookup"><span data-stu-id="18277-157">Editor Prelude</span></span>
<span data-ttu-id="18277-158">Egy szerkesztővel, amely támogatja az F # fordítóprogram szolgáltatások semmit sem éreznek hello névterek és-szerelvényt, amely automatikusan felveszi az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="18277-158">An editor that supports F# Compiler Services will not be aware of hello namespaces and assemblies that Azure Functions automatically includes.</span></span> <span data-ttu-id="18277-159">Ilyen lehet hasznos tooinclude egy prelude hello szerelvények használata található hello szerkesztő segítségével, és tooexplicitly nyissa meg a névterek.</span><span class="sxs-lookup"><span data-stu-id="18277-159">As such, it can be useful tooinclude a prelude that helps hello editor find hello assemblies you are using, and tooexplicitly open namespaces.</span></span> <span data-ttu-id="18277-160">Példa:</span><span class="sxs-lookup"><span data-stu-id="18277-160">For example:</span></span>

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

<span data-ttu-id="18277-161">A kód az Azure Functions végrehajtásakor hello adatforrással, amelynek feldolgozza `COMPILED` definiálva, ezért hello szerkesztő prelude figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="18277-161">When Azure Functions executes your code, it processes hello source with `COMPILED` defined, so hello editor prelude will be ignored.</span></span>

<a name="package"></a>

## <a name="package-management"></a><span data-ttu-id="18277-162">Felügyeleti csomag</span><span class="sxs-lookup"><span data-stu-id="18277-162">Package management</span></span>
<span data-ttu-id="18277-163">az F # függvényben, toouse NuGet-csomagok hozzáadása egy `project.json` toohello hello függvény mappa fájl hello függvény app fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="18277-163">toouse NuGet packages in an F# function, add a `project.json` file toohello hello function's folder in hello function app's file system.</span></span> <span data-ttu-id="18277-164">Példa `project.json` fájl, amely túl ad hozzá a NuGet csomag referenciája`Microsoft.ProjectOxford.Face` 1.1.0-ás verzió:</span><span class="sxs-lookup"><span data-stu-id="18277-164">Here is an example `project.json` file that adds a NuGet package reference too`Microsoft.ProjectOxford.Face` version 1.1.0:</span></span>

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

<span data-ttu-id="18277-165">Csak hello .NET-keretrendszer 4.6 támogatott, ezért győződjön meg arról, hogy a `project.json` fájl határozza meg `net46` itt látható módon.</span><span class="sxs-lookup"><span data-stu-id="18277-165">Only hello .NET Framework 4.6 is supported, so make sure that your `project.json` file specifies `net46` as shown here.</span></span>

<span data-ttu-id="18277-166">Feltöltése egy `project.json` fájlt, hello futásidejű hello csomagok lekérdezi és automatikusan hozzáadja a hivatkozások toohello csomag szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="18277-166">When you upload a `project.json` file, hello runtime gets hello packages and automatically adds references toohello package assemblies.</span></span> <span data-ttu-id="18277-167">Nem kell tooadd `#r "AssemblyName"` irányelvek.</span><span class="sxs-lookup"><span data-stu-id="18277-167">You don't need tooadd `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="18277-168">Csak adja hozzá a szükséges hello `open` utasítások tooyour `.fsx` fájlt.</span><span class="sxs-lookup"><span data-stu-id="18277-168">Just add hello required `open` statements tooyour `.fsx` file.</span></span>

<span data-ttu-id="18277-169">Kezdésként tooput automatikusan hivatkozik a szerkesztő prelude tooimprove szerelvényeket a szerkesztő interakcióba F # fordítási szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="18277-169">You may wish tooput automatically references assemblies in your editor prelude, tooimprove your editor's interaction with F# Compile Services.</span></span>

### <a name="how-tooadd-a-projectjson-file-tooyour-azure-function"></a><span data-ttu-id="18277-170">Hogyan tooadd egy `project.json` tooyour Azure-függvény fájl</span><span class="sxs-lookup"><span data-stu-id="18277-170">How tooadd a `project.json` file tooyour Azure Function</span></span>
1. <span data-ttu-id="18277-171">Először meggyőződött arról, hogy a függvény alkalmazás fut, amely el ellenőrzéséhez nyissa meg a függvény hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="18277-171">Begin by making sure your function app is running, which you can do by opening your function in hello Azure portal.</span></span> <span data-ttu-id="18277-172">Ez is hozzáférést toohello folyamatos átviteli naplók ahol csomag telepítési kimenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="18277-172">This also gives access toohello streaming logs where package installation output will be displayed.</span></span>
2. <span data-ttu-id="18277-173">tooupload egy `project.json` fájlt, az ismertetett hello módszerekkel [hogyan tooupdate működni az alkalmazás fájljai](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="18277-173">tooupload a `project.json` file, use one of hello methods described in [how tooupdate function app files](functions-reference.md#fileupdate).</span></span> <span data-ttu-id="18277-174">Használata [folyamatos üzembe helyezés az Azure Functions](functions-continuous-deployment.md), hozzáadhat egy `project.json` rendelés tooexperiment vele, mielőtt hozzáadná tooyour telepítési fiókirodai ágát átmeneti tooyour fájlt.</span><span class="sxs-lookup"><span data-stu-id="18277-174">If you are using [Continuous Deployment for Azure Functions](functions-continuous-deployment.md), you can add a `project.json` file tooyour staging branch in order tooexperiment with it before adding it tooyour deployment branch.</span></span>
3. <span data-ttu-id="18277-175">Hello után `project.json` fájl kerül, látni fogja, például a függvényben a következő kimeneti hasonló toohello tartozó streaming napló:</span><span class="sxs-lookup"><span data-stu-id="18277-175">After hello `project.json` file is added, you will see output similar toohello following example in your function's streaming log:</span></span>

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

## <a name="environment-variables"></a><span data-ttu-id="18277-176">Környezeti változók</span><span class="sxs-lookup"><span data-stu-id="18277-176">Environment variables</span></span>
<span data-ttu-id="18277-177">tooget egy környezeti változó vagy egy alkalmazás beállításérték, használjon `System.Environment.GetEnvironmentVariable`, például:</span><span class="sxs-lookup"><span data-stu-id="18277-177">tooget an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, for example:</span></span>

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a><span data-ttu-id="18277-178">Újbóli felhasználása .fsx kódot</span><span class="sxs-lookup"><span data-stu-id="18277-178">Reusing .fsx code</span></span>
<span data-ttu-id="18277-179">A többi kódot használhatja `.fsx` a fájlok egy `#load` direktívát.</span><span class="sxs-lookup"><span data-stu-id="18277-179">You can use code from other `.fsx` files by using a `#load` directive.</span></span> <span data-ttu-id="18277-180">Példa:</span><span class="sxs-lookup"><span data-stu-id="18277-180">For example:</span></span>

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

<span data-ttu-id="18277-181">Elérési utak biztosít toohello `#load` irányelv relatív toohello helyét a `.fsx` fájlt.</span><span class="sxs-lookup"><span data-stu-id="18277-181">Paths provides toohello `#load` directive are relative toohello location of your `.fsx` file.</span></span>

* <span data-ttu-id="18277-182">`#load "logger.fsx"`hello függvény mappában található fájl betöltése.</span><span class="sxs-lookup"><span data-stu-id="18277-182">`#load "logger.fsx"` loads a file located in hello function folder.</span></span>
* <span data-ttu-id="18277-183">`#load "package\logger.fsx"`hello található fájl betöltése `package` hello függvény mappája.</span><span class="sxs-lookup"><span data-stu-id="18277-183">`#load "package\logger.fsx"` loads a file located in hello `package` folder in hello function folder.</span></span>
* <span data-ttu-id="18277-184">`#load "..\shared\mylogger.fsx"`hello található fájl betöltése `shared` mappát a hello azonos szinten hello függvény mappaként Ez azt jelenti, hogy közvetlenül a `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="18277-184">`#load "..\shared\mylogger.fsx"` loads a file located in hello `shared` folder at hello same level as hello function folder, that is, directly under `wwwroot`.</span></span>

<span data-ttu-id="18277-185">Hello `#load` direktíva csak együttműködve `.fsx` (F # parancsfájl) fájlok, és nem a `.fs` fájlokat.</span><span class="sxs-lookup"><span data-stu-id="18277-185">hello `#load` directive only works with `.fsx` (F# script) files, and not with `.fs` files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18277-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="18277-186">Next steps</span></span>
<span data-ttu-id="18277-187">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="18277-187">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="18277-188">F # útmutató</span><span class="sxs-lookup"><span data-stu-id="18277-188">F# Guide</span></span>](/dotnet/articles/fsharp/index)
* [<span data-ttu-id="18277-189">Azure Functions – ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="18277-189">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="18277-190">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="18277-190">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="18277-191">Az Azure Functions C# fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="18277-191">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="18277-192">Az Azure Functions NodeJS fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="18277-192">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="18277-193">Az Azure Functions eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="18277-193">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* [<span data-ttu-id="18277-194">Az Azure Functions tesztelése</span><span class="sxs-lookup"><span data-stu-id="18277-194">Azure Functions testing</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="18277-195">Az Azure Functions méretezése</span><span class="sxs-lookup"><span data-stu-id="18277-195">Azure Functions scaling</span></span>](functions-scale.md)

