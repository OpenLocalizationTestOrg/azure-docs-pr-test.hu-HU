---
title: "Az Azure Functions F # fejlesztői leírás |} Microsoft Docs"
description: "Megtudhatja, hogyan fejleszthet Azure Functions használatával F #."
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
ms.openlocfilehash: 1691d378263f6b4ce5072f5c621d8db02f774b5f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-f-developer-reference"></a><span data-ttu-id="e4710-104">Az Azure Functions F # fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="e4710-104">Azure Functions F# Developer Reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e4710-105">C# parancsfájl</span><span class="sxs-lookup"><span data-stu-id="e4710-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="e4710-106">F # parancsfájl</span><span class="sxs-lookup"><span data-stu-id="e4710-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="e4710-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="e4710-107">Node.js</span></span>](functions-reference-node.md)
> 
> 

<span data-ttu-id="e4710-108">F # az Azure Functions egy megoldással egyszerűen futtathatók kisebb kódrészletek, más néven "függvények" a felhőben.</span><span class="sxs-lookup"><span data-stu-id="e4710-108">F# for Azure Functions is a solution for easily running small pieces of code, or "functions," in the cloud.</span></span> <span data-ttu-id="e4710-109">Az F # függvénynek keresztül Függvényargumentumok adatáramlás.</span><span class="sxs-lookup"><span data-stu-id="e4710-109">Data flows into your F# function via function arguments.</span></span> <span data-ttu-id="e4710-110">Argumentum neve meg van határozva a `function.json`, és nincsenek előre definiált nevek többek között a funkció naplózó és a megszakítási jogkivonatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e4710-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like the function logger and cancellation tokens.</span></span>

<span data-ttu-id="e4710-111">Ez a cikk feltételezi, hogy Ön már elolvasta a [Azure Functions fejlesztői segédanyagai](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="e4710-111">This article assumes that you've already read the [Azure Functions developer reference](functions-reference.md).</span></span>

## <a name="how-fsx-works"></a><span data-ttu-id="e4710-112">.Fsx működése</span><span class="sxs-lookup"><span data-stu-id="e4710-112">How .fsx works</span></span>
<span data-ttu-id="e4710-113">Egy `.fsx` fájl az F #-parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="e4710-113">An `.fsx` file is an F# script.</span></span> <span data-ttu-id="e4710-114">Akkor tekinthető az F # projekt egyetlen fájlban található.</span><span class="sxs-lookup"><span data-stu-id="e4710-114">It can be thought of as an F# project that's contained in a single file.</span></span> <span data-ttu-id="e4710-115">A fájl tartalmazza a kódban a program (ebben az esetben, az Azure-függvény) és irányelveket függőségek kezelése.</span><span class="sxs-lookup"><span data-stu-id="e4710-115">The file contains both the code for your program (in this case, your Azure Function) and directives for managing dependencies.</span></span>

<span data-ttu-id="e4710-116">Ha egy `.fsx` egy Azure-függvény, általában a szükséges szerelvényeket automatikusan érhetők el, ezáltal jobban összpontosíthat az "bolierplate" helyett a függvény a kódban.</span><span class="sxs-lookup"><span data-stu-id="e4710-116">When you use an `.fsx` for an Azure Function, commonly required assemblies are automatically included for you, allowing you to focus on the function rather than "boilerplate" code.</span></span>

## <a name="binding-to-arguments"></a><span data-ttu-id="e4710-117">Argumentum kötése</span><span class="sxs-lookup"><span data-stu-id="e4710-117">Binding to arguments</span></span>
<span data-ttu-id="e4710-118">Minden egyes kötés támogatja-e bizonyos argumentumok, ahogy az az készletét a [Azure Functions eseményindítók és kötések fejlesztői segédanyagai](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="e4710-118">Each binding supports some set of arguments, as detailed in the [Azure Functions triggers and bindings developer reference](functions-triggers-bindings.md).</span></span> <span data-ttu-id="e4710-119">Például egy blob eseményindító támogatja argumentum kötéseket egyik egy POCO, amelyek használatával az F # rekord jelöl.</span><span class="sxs-lookup"><span data-stu-id="e4710-119">For example, one of the argument bindings a blob trigger supports is a POCO, which can be expressed using an F# record.</span></span> <span data-ttu-id="e4710-120">Példa:</span><span class="sxs-lookup"><span data-stu-id="e4710-120">For example:</span></span>

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

<span data-ttu-id="e4710-121">Az F # Azure-függvény lépnek egy vagy több argumentum.</span><span class="sxs-lookup"><span data-stu-id="e4710-121">Your F# Azure Function will take one or more arguments.</span></span> <span data-ttu-id="e4710-122">Amikor döntésről bővebben az Azure Functions argumentumok, hivatkozunk *bemeneti* argumentumok és *kimeneti* argumentumok.</span><span class="sxs-lookup"><span data-stu-id="e4710-122">When we talk about Azure Functions arguments, we refer to *input* arguments and *output* arguments.</span></span> <span data-ttu-id="e4710-123">Egy bemeneti argumentum pontosan mit úgy tűnik, hogy például: az F # Azure-függvény a bemeneti.</span><span class="sxs-lookup"><span data-stu-id="e4710-123">An input argument is exactly what it sounds like: input to your F# Azure Function.</span></span> <span data-ttu-id="e4710-124">Egy *kimeneti* argumentum változtatható adatok vagy egy `byref<>` argumentumot, amelynek vissza az adatok úgy funkcionál *kimenő* a függvény.</span><span class="sxs-lookup"><span data-stu-id="e4710-124">An *output* argument is mutable data or a `byref<>` argument that serves as a way to pass data back *out* of your function.</span></span>

<span data-ttu-id="e4710-125">A példában `blob` egy bemeneti argumentum, és `output` kimeneti argumentum van.</span><span class="sxs-lookup"><span data-stu-id="e4710-125">In the example above, `blob` is an input argument, and `output` is an output argument.</span></span> <span data-ttu-id="e4710-126">Figyelje meg, hogy használtuk `byref<>` a `output` (nincs szükség hozzáadása a `[<Out>]` jegyzet).</span><span class="sxs-lookup"><span data-stu-id="e4710-126">Notice that we used `byref<>` for `output` (there's no need to add the `[<Out>]` annotation).</span></span> <span data-ttu-id="e4710-127">Használja a `byref<>` típus lehetővé teszi, hogy a függvény melyik rekord vagy argumentum a hivatkozott objektum módosítása.</span><span class="sxs-lookup"><span data-stu-id="e4710-127">Using a `byref<>` type allows your function to change which record or object the argument refers to.</span></span>

<span data-ttu-id="e4710-128">Az F # rekord bemeneti típusként használata esetén a rekord definition fel kell tüntetni az `[<CLIMutable>]` annak érdekében, hogy az Azure Functions keretrendszer a mezők megfelelően beállítása előtt a rekord a függvénynek.</span><span class="sxs-lookup"><span data-stu-id="e4710-128">When an F# record is used as an input type, the record definition must be marked with `[<CLIMutable>]` in order to allow the Azure Functions framework to set the fields appropriately before passing the record to your function.</span></span> <span data-ttu-id="e4710-129">A technikai részletek alatt `[<CLIMutable>]` állít elő, a rekord tulajdonságok Setter elemek.</span><span class="sxs-lookup"><span data-stu-id="e4710-129">Under the hood, `[<CLIMutable>]` generates setters for the record properties.</span></span> <span data-ttu-id="e4710-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="e4710-130">For example:</span></span>

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

<span data-ttu-id="e4710-131">Az F # osztály mindkét és argumentumokat is használható.</span><span class="sxs-lookup"><span data-stu-id="e4710-131">An F# class can also be used for both in and out arguments.</span></span> <span data-ttu-id="e4710-132">Egy osztály tulajdonságok általában kell beolvasókat és Setter elemek.</span><span class="sxs-lookup"><span data-stu-id="e4710-132">For a class, properties will usually need getters and setters.</span></span> <span data-ttu-id="e4710-133">Példa:</span><span class="sxs-lookup"><span data-stu-id="e4710-133">For example:</span></span>

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a><span data-ttu-id="e4710-134">Naplózás</span><span class="sxs-lookup"><span data-stu-id="e4710-134">Logging</span></span>
<span data-ttu-id="e4710-135">A kimeneti bejelentkezni a [a folyamatos átviteli naplók](../app-service-web/web-sites-streaming-logs-and-console.md) F #, a függvény típusú argumentumot kell venniük `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="e4710-135">To log output to your [streaming logs](../app-service-web/web-sites-streaming-logs-and-console.md) in F#, your function should take an argument of type `TraceWriter`.</span></span> <span data-ttu-id="e4710-136">A konzisztencia, javasoljuk, ez az argumentum neve `log`.</span><span class="sxs-lookup"><span data-stu-id="e4710-136">For consistency, we recommend this argument is named `log`.</span></span> <span data-ttu-id="e4710-137">Példa:</span><span class="sxs-lookup"><span data-stu-id="e4710-137">For example:</span></span>

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a><span data-ttu-id="e4710-138">Aszinkron</span><span class="sxs-lookup"><span data-stu-id="e4710-138">Async</span></span>
<span data-ttu-id="e4710-139">A `async` munkafolyamat is használható, de az eredmény vissza kell adnia egy `Task`.</span><span class="sxs-lookup"><span data-stu-id="e4710-139">The `async` workflow can be used, but the result needs to return a `Task`.</span></span> <span data-ttu-id="e4710-140">Ezt megteheti a `Async.StartAsTask`, például:</span><span class="sxs-lookup"><span data-stu-id="e4710-140">This can be done with `Async.StartAsTask`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a><span data-ttu-id="e4710-141">Token törlése</span><span class="sxs-lookup"><span data-stu-id="e4710-141">Cancellation Token</span></span>
<span data-ttu-id="e4710-142">Ha a függvény leállítási szabályosan kezelni, amelynek megadhatja egy [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentum.</span><span class="sxs-lookup"><span data-stu-id="e4710-142">If your function needs to handle shutdown gracefully, you can give it a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argument.</span></span> <span data-ttu-id="e4710-143">Ez kombinálva `async`, például:</span><span class="sxs-lookup"><span data-stu-id="e4710-143">This can be combined with `async`, for example:</span></span>

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a><span data-ttu-id="e4710-144">Névterek importálása</span><span class="sxs-lookup"><span data-stu-id="e4710-144">Importing namespaces</span></span>
<span data-ttu-id="e4710-145">Névterek megnyithatók a szokásos módon:</span><span class="sxs-lookup"><span data-stu-id="e4710-145">Namespaces can be opened in the usual way:</span></span>

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="e4710-146">A következő névterek automatikusan megnyitása:</span><span class="sxs-lookup"><span data-stu-id="e4710-146">The following namespaces are automatically opened:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* <span data-ttu-id="e4710-147">`Microsoft.Azure.WebJobs.Host`.</span><span class="sxs-lookup"><span data-stu-id="e4710-147">`Microsoft.Azure.WebJobs.Host`.</span></span>

## <a name="referencing-external-assemblies"></a><span data-ttu-id="e4710-148">Külső szerelvények hivatkozik</span><span class="sxs-lookup"><span data-stu-id="e4710-148">Referencing External Assemblies</span></span>
<span data-ttu-id="e4710-149">Hasonlóképpen, a keretrendszer szerelvény hivatkozások adhatók hozzá, amelyeknél a `#r "AssemblyName"` direktívát.</span><span class="sxs-lookup"><span data-stu-id="e4710-149">Similarly, framework assembly references be added with the `#r "AssemblyName"` directive.</span></span>

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

<span data-ttu-id="e4710-150">A következő szerelvények a rendszer automatikusan hozzáadja a üzemeltetési környezet az Azure Functions:</span><span class="sxs-lookup"><span data-stu-id="e4710-150">The following assemblies are automatically added by the Azure Functions hosting environment:</span></span>

* <span data-ttu-id="e4710-151">`mscorlib`,</span><span class="sxs-lookup"><span data-stu-id="e4710-151">`mscorlib`,</span></span>
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* <span data-ttu-id="e4710-152">`System.Net.Http.Formatting`.</span><span class="sxs-lookup"><span data-stu-id="e4710-152">`System.Net.Http.Formatting`.</span></span>

<span data-ttu-id="e4710-153">Ezenkívül a következő szerelvényekben különleges cased és simplename is hivatkozhat (pl. `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="e4710-153">In addition, the following assemblies are special cased and may be referenced by simplename (e.g. `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* <span data-ttu-id="e4710-154">`Microsoft.AspNEt.WebHooks.Common`.</span><span class="sxs-lookup"><span data-stu-id="e4710-154">`Microsoft.AspNEt.WebHooks.Common`.</span></span>

<span data-ttu-id="e4710-155">A belső szerelvény hivatkoznia kell, ha a szerelvény fájlból feltöltheti egy `bin` mappa viszonyítva az függvény és a hivatkozási (pl. name, a fájl használatával  `#r "MyAssembly.dll"`).</span><span class="sxs-lookup"><span data-stu-id="e4710-155">If you need to reference a private assembly, you can upload the assembly file into a `bin` folder relative to your function and reference it by using the file name (e.g.  `#r "MyAssembly.dll"`).</span></span> <span data-ttu-id="e4710-156">A fájlok feltöltéséről a függvény mappába információkért lásd a következő felügyeleti csomag.</span><span class="sxs-lookup"><span data-stu-id="e4710-156">For information on how to upload files to your function folder, see the following section on package management.</span></span>

## <a name="editor-prelude"></a><span data-ttu-id="e4710-157">Szerkesztő Prelude</span><span class="sxs-lookup"><span data-stu-id="e4710-157">Editor Prelude</span></span>
<span data-ttu-id="e4710-158">Egy szerkesztővel, amely támogatja az F # fordítóprogram szolgáltatások semmit sem éreznek a névterek és-szerelvényt, amely automatikusan felveszi az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="e4710-158">An editor that supports F# Compiler Services will not be aware of the namespaces and assemblies that Azure Functions automatically includes.</span></span> <span data-ttu-id="e4710-159">Ilyen hasznos lehet egy prelude található használja a szerelvényeket a szerkesztő segítségével, és explicit módon a névterek megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e4710-159">As such, it can be useful to include a prelude that helps the editor find the assemblies you are using, and to explicitly open namespaces.</span></span> <span data-ttu-id="e4710-160">Példa:</span><span class="sxs-lookup"><span data-stu-id="e4710-160">For example:</span></span>

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

<span data-ttu-id="e4710-161">A kód az Azure Functions végrehajtásakor feldolgozza az adatforrással, amelynek `COMPILED` definiálva, ezért figyelmen kívül hagyja a szerkesztő prelude.</span><span class="sxs-lookup"><span data-stu-id="e4710-161">When Azure Functions executes your code, it processes the source with `COMPILED` defined, so the editor prelude will be ignored.</span></span>

<a name="package"></a>

## <a name="package-management"></a><span data-ttu-id="e4710-162">Felügyeleti csomag</span><span class="sxs-lookup"><span data-stu-id="e4710-162">Package management</span></span>
<span data-ttu-id="e4710-163">Az F # függvény NuGet-csomagok használatához vegye fel a `project.json` fájlt a a függvény mappa, a függvény app fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="e4710-163">To use NuGet packages in an F# function, add a `project.json` file to the the function's folder in the function app's file system.</span></span> <span data-ttu-id="e4710-164">Példa `project.json` hozzáad egy NuGet csomag hivatkozást tartalmazó `Microsoft.ProjectOxford.Face` 1.1.0-ás verzió:</span><span class="sxs-lookup"><span data-stu-id="e4710-164">Here is an example `project.json` file that adds a NuGet package reference to `Microsoft.ProjectOxford.Face` version 1.1.0:</span></span>

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

<span data-ttu-id="e4710-165">Csak a .NET keretrendszer 4.6-os támogatott, ezért győződjön meg arról, hogy a `project.json` fájl határozza meg `net46` itt látható módon.</span><span class="sxs-lookup"><span data-stu-id="e4710-165">Only the .NET Framework 4.6 is supported, so make sure that your `project.json` file specifies `net46` as shown here.</span></span>

<span data-ttu-id="e4710-166">Feltöltése egy `project.json` fájl, a futtatókörnyezet lekérdezi a csomagokat, és automatikusan hozzáadja a csomag szerelvények hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e4710-166">When you upload a `project.json` file, the runtime gets the packages and automatically adds references to the package assemblies.</span></span> <span data-ttu-id="e4710-167">Nem kell hozzáadnia `#r "AssemblyName"` irányelvek.</span><span class="sxs-lookup"><span data-stu-id="e4710-167">You don't need to add `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="e4710-168">A szükséges hozzá `open` utasítást, hogy a `.fsx` fájlt.</span><span class="sxs-lookup"><span data-stu-id="e4710-168">Just add the required `open` statements to your `.fsx` file.</span></span>

<span data-ttu-id="e4710-169">Kezdésként érdemes lehet helyezni a szerkesztő prelude, F # lefordítani a szolgáltatások a szerkesztő interakcióba javítása érdekében automatikusan hivatkozásokat szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="e4710-169">You may wish to put automatically references assemblies in your editor prelude, to improve your editor's interaction with F# Compile Services.</span></span>

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a><span data-ttu-id="e4710-170">Hogyan lehet hozzáadni egy `project.json` fájlt az Azure-függvény</span><span class="sxs-lookup"><span data-stu-id="e4710-170">How to add a `project.json` file to your Azure Function</span></span>
1. <span data-ttu-id="e4710-171">Először meggyőződött arról, hogy a függvény alkalmazás fut, amely a függvény nyissa meg az Azure portálon teheti.</span><span class="sxs-lookup"><span data-stu-id="e4710-171">Begin by making sure your function app is running, which you can do by opening your function in the Azure portal.</span></span> <span data-ttu-id="e4710-172">Ez is hozzáférést biztosít a folyamatos átviteli naplók ahol csomag telepítési kimenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e4710-172">This also gives access to the streaming logs where package installation output will be displayed.</span></span>
2. <span data-ttu-id="e4710-173">Töltse fel a `project.json` fájlt, az ismertetett módszerek egyikével [függvény alkalmazásfájlok frissítése](functions-reference.md#fileupdate).</span><span class="sxs-lookup"><span data-stu-id="e4710-173">To upload a `project.json` file, use one of the methods described in [how to update function app files](functions-reference.md#fileupdate).</span></span> <span data-ttu-id="e4710-174">Ha használ [folyamatos üzembe helyezés az Azure Functions](functions-continuous-deployment.md), adhat hozzá egy `project.json` ahhoz, hogy mielőtt hozzáadná a központi telepítés fiókirodai kísérletezhet az átmeneti ágában fájlt.</span><span class="sxs-lookup"><span data-stu-id="e4710-174">If you are using [Continuous Deployment for Azure Functions](functions-continuous-deployment.md), you can add a `project.json` file to your staging branch in order to experiment with it before adding it to your deployment branch.</span></span>
3. <span data-ttu-id="e4710-175">Miután a `project.json` fájl kerül, látni fogja, a függvény az alábbi példához hasonló kimenetet a streaming naplóban:</span><span class="sxs-lookup"><span data-stu-id="e4710-175">After the `project.json` file is added, you will see output similar to the following example in your function's streaming log:</span></span>

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

## <a name="environment-variables"></a><span data-ttu-id="e4710-176">Környezeti változók</span><span class="sxs-lookup"><span data-stu-id="e4710-176">Environment variables</span></span>
<span data-ttu-id="e4710-177">Egy környezeti változó vagy olyan alkalmazás, beállítás értékét, amelyet `System.Environment.GetEnvironmentVariable`, például:</span><span class="sxs-lookup"><span data-stu-id="e4710-177">To get an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, for example:</span></span>

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a><span data-ttu-id="e4710-178">Újbóli felhasználása .fsx kódot</span><span class="sxs-lookup"><span data-stu-id="e4710-178">Reusing .fsx code</span></span>
<span data-ttu-id="e4710-179">A többi kódot használhatja `.fsx` a fájlok egy `#load` direktívát.</span><span class="sxs-lookup"><span data-stu-id="e4710-179">You can use code from other `.fsx` files by using a `#load` directive.</span></span> <span data-ttu-id="e4710-180">Példa:</span><span class="sxs-lookup"><span data-stu-id="e4710-180">For example:</span></span>

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

<span data-ttu-id="e4710-181">Elérési utak biztosít a `#load` helyéhez viszonyított irányelv vannak a `.fsx` fájlt.</span><span class="sxs-lookup"><span data-stu-id="e4710-181">Paths provides to the `#load` directive are relative to the location of your `.fsx` file.</span></span>

* <span data-ttu-id="e4710-182">`#load "logger.fsx"`a függvény mappában található fájl betöltése.</span><span class="sxs-lookup"><span data-stu-id="e4710-182">`#load "logger.fsx"` loads a file located in the function folder.</span></span>
* <span data-ttu-id="e4710-183">`#load "package\logger.fsx"`található fájl betöltése a `package` mappája a függvény.</span><span class="sxs-lookup"><span data-stu-id="e4710-183">`#load "package\logger.fsx"` loads a file located in the `package` folder in the function folder.</span></span>
* <span data-ttu-id="e4710-184">`#load "..\shared\mylogger.fsx"`található fájl betöltése a `shared` mappaként a funkciót, ez azt jelenti, hogy ugyanazon a szinten mappát közvetlenül a `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="e4710-184">`#load "..\shared\mylogger.fsx"` loads a file located in the `shared` folder at the same level as the function folder, that is, directly under `wwwroot`.</span></span>

<span data-ttu-id="e4710-185">A `#load` direktíva csak együttműködve `.fsx` (F # parancsfájl) fájlok, és nem a `.fs` fájlokat.</span><span class="sxs-lookup"><span data-stu-id="e4710-185">The `#load` directive only works with `.fsx` (F# script) files, and not with `.fs` files.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4710-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4710-186">Next steps</span></span>
<span data-ttu-id="e4710-187">További információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="e4710-187">For more information, see the following resources:</span></span>

* [<span data-ttu-id="e4710-188">F # útmutató</span><span class="sxs-lookup"><span data-stu-id="e4710-188">F# Guide</span></span>](/dotnet/articles/fsharp/index)
* [<span data-ttu-id="e4710-189">Azure Functions – ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="e4710-189">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="e4710-190">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="e4710-190">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="e4710-191">Az Azure Functions C# fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="e4710-191">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="e4710-192">Az Azure Functions NodeJS fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="e4710-192">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="e4710-193">Az Azure Functions eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="e4710-193">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* [<span data-ttu-id="e4710-194">Az Azure Functions tesztelése</span><span class="sxs-lookup"><span data-stu-id="e4710-194">Azure Functions testing</span></span>](functions-test-a-function.md)
* [<span data-ttu-id="e4710-195">Az Azure Functions méretezése</span><span class="sxs-lookup"><span data-stu-id="e4710-195">Azure Functions scaling</span></span>](functions-scale.md)

