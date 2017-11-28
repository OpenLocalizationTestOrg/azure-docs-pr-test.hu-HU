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
# <a name="azure-functions-c-script-developer-reference"></a><span data-ttu-id="7ce06-104">Az Azure parancsfájl fejlesztői leírás funkciók C#</span><span class="sxs-lookup"><span data-stu-id="7ce06-104">Azure Functions C# script developer reference</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7ce06-105">C# parancsfájl</span><span class="sxs-lookup"><span data-stu-id="7ce06-105">C# script</span></span>](functions-reference-csharp.md)
> * [<span data-ttu-id="7ce06-106">F # parancsfájl</span><span class="sxs-lookup"><span data-stu-id="7ce06-106">F# script</span></span>](functions-reference-fsharp.md)
> * [<span data-ttu-id="7ce06-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="7ce06-107">Node.js</span></span>](functions-reference-node.md)
>
>

<span data-ttu-id="7ce06-108">C# parancsfájl tapasztalattal az Azure Functions hello hello Azure WebJobs SDK alapul.</span><span class="sxs-lookup"><span data-stu-id="7ce06-108">hello C# script experience for Azure Functions is based on hello Azure WebJobs SDK.</span></span> <span data-ttu-id="7ce06-109">Az adatáramlás a C# függvénynek keresztül metódus argumentumait.</span><span class="sxs-lookup"><span data-stu-id="7ce06-109">Data flows into your C# function via method arguments.</span></span> <span data-ttu-id="7ce06-110">Argumentum neve meg van határozva a `function.json`, és nincsenek előre definiált nevek mint függvény naplózó és a megszakítási jogkivonatok hello eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="7ce06-110">Argument names are specified in `function.json`, and there are predefined names for accessing things like hello function logger and cancellation tokens.</span></span>

<span data-ttu-id="7ce06-111">Ez a cikk feltételezi, hogy Ön már elolvasta hello [Azure Functions fejlesztői segédanyagai](functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="7ce06-111">This article assumes that you've already read hello [Azure Functions developer reference](functions-reference.md).</span></span>

<span data-ttu-id="7ce06-112">Információk az C# osztály szalagtárak című [használó alkalmazás az Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="7ce06-112">For information on using C# class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span>

## <a name="how-csx-works"></a><span data-ttu-id="7ce06-113">.Csx működése</span><span class="sxs-lookup"><span data-stu-id="7ce06-113">How .csx works</span></span>
<span data-ttu-id="7ce06-114">Hello `.csx` formátum lehetővé teszi a toowrite kevesebb mint "bolierplate" és a csak egy C# függvény írása.</span><span class="sxs-lookup"><span data-stu-id="7ce06-114">hello `.csx` format allows you toowrite less "boilerplate" and focus on writing just a C# function.</span></span> <span data-ttu-id="7ce06-115">Például bármely szerelvényhivatkozások és a névterek hello fájl hello elején a szokásos módon.</span><span class="sxs-lookup"><span data-stu-id="7ce06-115">Include any assembly references and namespaces at hello beginning of hello file as usual.</span></span> <span data-ttu-id="7ce06-116">Ahelyett, hogy a névtér és osztály alkalmazásburkoló mindent, csak adja meg a `Run` metódust.</span><span class="sxs-lookup"><span data-stu-id="7ce06-116">Instead of wrapping everything in a namespace and class, just define a `Run` method.</span></span> <span data-ttu-id="7ce06-117">Ha tooinclude bármely osztályok, a példány toodefine egyszerű régi CLR-objektum (POCO) objektumok, megadhat egy osztály belsejében hello ugyanazt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce06-117">If you need tooinclude any classes, for instance toodefine Plain Old CLR Object (POCO) objects, you can include a class inside hello same file.</span></span>   

## <a name="binding-tooarguments"></a><span data-ttu-id="7ce06-118">Kötési tooarguments</span><span class="sxs-lookup"><span data-stu-id="7ce06-118">Binding tooarguments</span></span>
<span data-ttu-id="7ce06-119">hello különböző kötések kötött tooa C# függvény keresztül hello `name` hello tulajdonság *function.json* konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="7ce06-119">hello various bindings are bound tooa C# function via hello `name` property in hello *function.json* configuration.</span></span> <span data-ttu-id="7ce06-120">Minden kötésnek rendelkezik saját támogatott típusok; egy blob eseményindító támogathat például egy karakterlánc, egy POCO vagy egy CloudBlockBlob.</span><span class="sxs-lookup"><span data-stu-id="7ce06-120">Each binding has its own supported types; for instance, a blob trigger can support a string, a POCO, or a CloudBlockBlob.</span></span> <span data-ttu-id="7ce06-121">hello támogatott típusok a kötésben hello útmutatóban szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="7ce06-121">hello supported types are documented in hello reference for each binding.</span></span> <span data-ttu-id="7ce06-122">Egy POCO objektumot hozzá elérő és beállító mindegyik tulajdonsághoz megadott kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7ce06-122">A POCO object must have a getter and setter defined for each property.</span></span>

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

## <a name="using-method-return-value-for-output-binding"></a><span data-ttu-id="7ce06-123">A kimeneti kötés metódus visszatérési értéke használatával</span><span class="sxs-lookup"><span data-stu-id="7ce06-123">Using method return value for output binding</span></span>

<span data-ttu-id="7ce06-124">Használhatja a metódus visszatérési érték egy kimeneti kötése hello nevének megadásával `$return` a *function.json*:</span><span class="sxs-lookup"><span data-stu-id="7ce06-124">You can use a method return value for an output binding, by using hello name `$return` in *function.json*:</span></span>

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

## <a name="writing-multiple-output-values"></a><span data-ttu-id="7ce06-125">Több kimeneti értékeinek írása</span><span class="sxs-lookup"><span data-stu-id="7ce06-125">Writing multiple output values</span></span>

<span data-ttu-id="7ce06-126">toowrite több értékek tooan kimeneti kötése, használja a hello [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) vagy [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) típusok.</span><span class="sxs-lookup"><span data-stu-id="7ce06-126">toowrite multiple values tooan output binding, use hello [`ICollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) or [`IAsyncCollector`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) types.</span></span> <span data-ttu-id="7ce06-127">Ezek a típusok csak írható gyűjtemények írásbeli toohello hello metódus befejezésekor a kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="7ce06-127">These types are write-only collections that are written toohello output binding when hello method completes.</span></span>

<span data-ttu-id="7ce06-128">Ez a példa ír használatával több üzenetek `ICollector`:</span><span class="sxs-lookup"><span data-stu-id="7ce06-128">This example writes multiple queue messages using `ICollector`:</span></span>

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a><span data-ttu-id="7ce06-129">Naplózás</span><span class="sxs-lookup"><span data-stu-id="7ce06-129">Logging</span></span>
<span data-ttu-id="7ce06-130">toolog kimeneti tooyour folyamatos átviteli naplók C# nyelven íródtak, típusú argumentumot tartalmaz `TraceWriter`.</span><span class="sxs-lookup"><span data-stu-id="7ce06-130">toolog output tooyour streaming logs in C#, include an argument of type `TraceWriter`.</span></span> <span data-ttu-id="7ce06-131">Ajánlott nevezze el `log`.</span><span class="sxs-lookup"><span data-stu-id="7ce06-131">We recommend that you name it `log`.</span></span> <span data-ttu-id="7ce06-132">Kerülje a `Console.Write` az Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="7ce06-132">Avoid using `Console.Write` in Azure Functions.</span></span> 

<span data-ttu-id="7ce06-133">`TraceWriter`hello meghatározott [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs).</span><span class="sxs-lookup"><span data-stu-id="7ce06-133">`TraceWriter` is defined in hello [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs).</span></span> <span data-ttu-id="7ce06-134">a naplózási szint hello `TraceWriter` konfigurálható [állomás\.json].</span><span class="sxs-lookup"><span data-stu-id="7ce06-134">hello log level for `TraceWriter` can be configured in [host\.json].</span></span>

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a><span data-ttu-id="7ce06-135">Aszinkron</span><span class="sxs-lookup"><span data-stu-id="7ce06-135">Async</span></span>
<span data-ttu-id="7ce06-136">egy aszinkron függvény toomake hello használata `async` kulcsszót, és térjen vissza a `Task` objektum.</span><span class="sxs-lookup"><span data-stu-id="7ce06-136">toomake a function asynchronous, use hello `async` keyword and return a `Task` object.</span></span>

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a><span data-ttu-id="7ce06-137">Token törlése</span><span class="sxs-lookup"><span data-stu-id="7ce06-137">Cancellation Token</span></span>
<span data-ttu-id="7ce06-138">Egyes műveletek szabályos leállítást igényel.</span><span class="sxs-lookup"><span data-stu-id="7ce06-138">Some operations require graceful shutdown.</span></span> <span data-ttu-id="7ce06-139">Amíg a rendszer mindig ajánlott toowrite kódot, amelyet kezelni tud összeomló toohandle szabályos leállítást kérelmeket, ahová esetekben adhat a [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) megadott argumentum.</span><span class="sxs-lookup"><span data-stu-id="7ce06-139">While it's always best toowrite code that can handle crashing,  in cases where you want toohandle graceful shutdown requests, you define a [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) typed argument.</span></span>  <span data-ttu-id="7ce06-140">A `CancellationToken` valósul meg, hogy a gazdagép leállítása kiváltásáról toosignal.</span><span class="sxs-lookup"><span data-stu-id="7ce06-140">A `CancellationToken` is provided toosignal that a host shutdown is triggered.</span></span>

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

## <a name="importing-namespaces"></a><span data-ttu-id="7ce06-141">Névterek importálása</span><span class="sxs-lookup"><span data-stu-id="7ce06-141">Importing namespaces</span></span>
<span data-ttu-id="7ce06-142">Ha tooimport névterek van szüksége, ehhez a szokásos módon, a hello `using` záradékban.</span><span class="sxs-lookup"><span data-stu-id="7ce06-142">If you need tooimport namespaces, you can do so as usual, with hello `using` clause.</span></span>

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

<span data-ttu-id="7ce06-143">hello következő névterek automatikusan importált, ezért nem kötelező:</span><span class="sxs-lookup"><span data-stu-id="7ce06-143">hello following namespaces are automatically imported and are therefore optional:</span></span>

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a><span data-ttu-id="7ce06-144">Külső szerelvények hivatkozik</span><span class="sxs-lookup"><span data-stu-id="7ce06-144">Referencing External Assemblies</span></span>
<span data-ttu-id="7ce06-145">A keretrendszer szerelvényei közé, hivatkozások hozzáadása hello segítségével `#r "AssemblyName"` direktívát.</span><span class="sxs-lookup"><span data-stu-id="7ce06-145">For framework assemblies, add references by using hello `#r "AssemblyName"` directive.</span></span>

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

<span data-ttu-id="7ce06-146">hello következő szerelvények automatikusan hello az Azure Functions Gazdakörnyezet:</span><span class="sxs-lookup"><span data-stu-id="7ce06-146">hello following assemblies are automatically added by hello Azure Functions hosting environment:</span></span>

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

<span data-ttu-id="7ce06-147">hello következő szerelvényekben lehet hivatkozni egyszerű-név szerint (például `#r "AssemblyName"`):</span><span class="sxs-lookup"><span data-stu-id="7ce06-147">hello following assemblies may be referenced by simple-name (for example, `#r "AssemblyName"`):</span></span>

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a><span data-ttu-id="7ce06-148">Egyéni szerelvényeket hivatkozik</span><span class="sxs-lookup"><span data-stu-id="7ce06-148">Referencing custom assemblies</span></span>

<span data-ttu-id="7ce06-149">egy egyéni szerelvény tooreference, használhatja a *megosztott* szerelvény vagy egy *titkos* szerelvény:</span><span class="sxs-lookup"><span data-stu-id="7ce06-149">tooreference a custom assembly, you can use either a *shared* assembly or a *private* assembly:</span></span>
- <span data-ttu-id="7ce06-150">Közös szerelvényekre összes funkciók, a függvény alkalmazások között vannak megosztva.</span><span class="sxs-lookup"><span data-stu-id="7ce06-150">Shared assemblies are shared across all functions within a function app.</span></span> <span data-ttu-id="7ce06-151">egy egyéni szerelvény tooreference hello szerelvény tooyour függvény alkalmazás feltöltése, többek között a egy `bin` hello függvény alkalmazás legfelső szintű mappa.</span><span class="sxs-lookup"><span data-stu-id="7ce06-151">tooreference a custom assembly, upload hello assembly tooyour function app, such as in a `bin` folder in hello function app root.</span></span> 
- <span data-ttu-id="7ce06-152">Titkos szerelvények egy adott funkció környezet részét képezik, és a tesztcélú különböző verzióit támogatja.</span><span class="sxs-lookup"><span data-stu-id="7ce06-152">Private assemblies are part of a given function's context, and support side-loading of different versions.</span></span> <span data-ttu-id="7ce06-153">Személyes szerelvények fel kell tölteni a egy `bin` hello függvény directory mappájában.</span><span class="sxs-lookup"><span data-stu-id="7ce06-153">Private assemblies should be uploaded in a `bin` folder in hello function directory.</span></span> <span data-ttu-id="7ce06-154">Hivatkozás hello fájl nevét, például használatával `#r "MyAssembly.dll"`.</span><span class="sxs-lookup"><span data-stu-id="7ce06-154">Reference using hello file name, such as  `#r "MyAssembly.dll"`.</span></span> 

<span data-ttu-id="7ce06-155">Hogyan tooupload fájlmappa tooyour függvény a további információkért lásd: a következő szakasz a felügyeleti csomag hello.</span><span class="sxs-lookup"><span data-stu-id="7ce06-155">For information on how tooupload files tooyour function folder, see hello following section on package management.</span></span>

### <a name="watched-directories"></a><span data-ttu-id="7ce06-156">Figyelt könyvtárak</span><span class="sxs-lookup"><span data-stu-id="7ce06-156">Watched directories</span></span>

<span data-ttu-id="7ce06-157">hello függvény parancsfájlt tartalmazó hello könyvtár automatikusan figyelt a módosítások tooassemblies.</span><span class="sxs-lookup"><span data-stu-id="7ce06-157">hello directory that contains hello function script file is automatically watched for changes tooassemblies.</span></span> <span data-ttu-id="7ce06-158">a szerelvény változásokat más címtárakban toowatch toohello adja hozzá `watchDirectories` listájában [állomás\.json].</span><span class="sxs-lookup"><span data-stu-id="7ce06-158">toowatch for assembly changes in other directories, add them toohello `watchDirectories` list in [host\.json].</span></span>

## <a name="using-nuget-packages"></a><span data-ttu-id="7ce06-159">NuGet-csomagok használata</span><span class="sxs-lookup"><span data-stu-id="7ce06-159">Using NuGet packages</span></span>
<span data-ttu-id="7ce06-160">NuGet-csomagok toouse C# függvények, töltse fel a *project.json* toohello függvény mappa fájl hello függvény app fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="7ce06-160">toouse NuGet packages in a C# function, upload a *project.json* file toohello function's folder in hello function app's file system.</span></span> <span data-ttu-id="7ce06-161">Példa *project.json* fájl, amely ad hozzá egy hivatkozást tooMicrosoft.ProjectOxford.Face verzió 1.1.0-ás:</span><span class="sxs-lookup"><span data-stu-id="7ce06-161">Here is an example *project.json* file that adds a reference tooMicrosoft.ProjectOxford.Face version 1.1.0:</span></span>

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

<span data-ttu-id="7ce06-162">Csak hello .NET-keretrendszer 4.6 támogatott, ezért győződjön meg arról, hogy a *project.json* fájl határozza meg `net46` itt látható módon.</span><span class="sxs-lookup"><span data-stu-id="7ce06-162">Only hello .NET Framework 4.6 is supported, so make sure that your *project.json* file specifies `net46` as shown here.</span></span>

<span data-ttu-id="7ce06-163">Feltöltése egy *project.json* fájlt, hello futásidejű hello csomagok lekérdezi és automatikusan hozzáadja a hivatkozások toohello csomag szerelvényeket.</span><span class="sxs-lookup"><span data-stu-id="7ce06-163">When you upload a *project.json* file, hello runtime gets hello packages and automatically adds references toohello package assemblies.</span></span> <span data-ttu-id="7ce06-164">Nem kell tooadd `#r "AssemblyName"` irányelvek.</span><span class="sxs-lookup"><span data-stu-id="7ce06-164">You don't need tooadd `#r "AssemblyName"` directives.</span></span> <span data-ttu-id="7ce06-165">hello NuGet-csomagok definiált toouse hello típusok hozzáadása szükséges hello `using` utasítások tooyour *run.csx* fájl</span><span class="sxs-lookup"><span data-stu-id="7ce06-165">toouse hello types defined in hello NuGet packages, add hello required `using` statements tooyour *run.csx* file</span></span> 

<span data-ttu-id="7ce06-166">Összehasonlításával működik NuGet visszaállítási hello Functions futtatókörnyezete, `project.json` és `project.lock.json`.</span><span class="sxs-lookup"><span data-stu-id="7ce06-166">In hello Functions runtime, NuGet restore works by comparing `project.json` and `project.lock.json`.</span></span> <span data-ttu-id="7ce06-167">Ha dátum és idő bélyegzők hello fájlok hello **nem** egyezik, a NuGet-visszaállítás fut, és a NuGet-letöltések csomagok frissítése.</span><span class="sxs-lookup"><span data-stu-id="7ce06-167">If hello date and time stamps of hello files **do not** match, a NuGet restore runs and NuGet downloads updated packages.</span></span> <span data-ttu-id="7ce06-168">Azonban, ha hello dátum és idő bélyegzők hello fájlok **tegye** egyezik, NuGet, a visszaállítás nem hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="7ce06-168">However, if hello date and time stamps of hello files **do** match, NuGet does not perform a restore.</span></span> <span data-ttu-id="7ce06-169">Ezért `project.lock.json` nem kell telepíteni, mivel azt eredményezi, hogy a NuGet tooskip csomag visszaállítási.</span><span class="sxs-lookup"><span data-stu-id="7ce06-169">Therefore, `project.lock.json` should not be deployed as it causes NuGet tooskip package restore.</span></span> <span data-ttu-id="7ce06-170">hello zárolási telepítése tooavoid fájlt, adja hozzá a hello `project.lock.json` toohello `.gitignore` fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce06-170">tooavoid deploying hello lock file, add hello `project.lock.json` toohello `.gitignore` file.</span></span>

<span data-ttu-id="7ce06-171">egyéni NuGet-hírcsatorna toouse adja meg az adatcsatorna hello egy *Nuget.Config* hello függvény alkalmazás legfelső szintű fájl.</span><span class="sxs-lookup"><span data-stu-id="7ce06-171">toouse a custom NuGet feed, specify hello feed in a *Nuget.Config* file in hello Function App root.</span></span> <span data-ttu-id="7ce06-172">További információkért lásd: [konfigurálása NuGet viselkedés](/nuget/consume-packages/configuring-nuget-behavior).</span><span class="sxs-lookup"><span data-stu-id="7ce06-172">For more information, see [Configuring NuGet behavior](/nuget/consume-packages/configuring-nuget-behavior).</span></span>

### <a name="using-a-projectjson-file"></a><span data-ttu-id="7ce06-173">Project.json fájl használatával</span><span class="sxs-lookup"><span data-stu-id="7ce06-173">Using a project.json file</span></span>
1. <span data-ttu-id="7ce06-174">Nyissa meg a hello függvény hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7ce06-174">Open hello function in hello Azure portal.</span></span> <span data-ttu-id="7ce06-175">hello lapon megjeleníti hello csomag telepítési kimeneti naplózza.</span><span class="sxs-lookup"><span data-stu-id="7ce06-175">hello logs tab displays hello package installation output.</span></span>
2. <span data-ttu-id="7ce06-176">egy project.json fájl tooupload hello ismertetett hello módszerek valamelyikével [hogyan tooupdate működni az alkalmazás fájljai](functions-reference.md#fileupdate) hello Azure Functions fejlesztői referencia-témakör a.</span><span class="sxs-lookup"><span data-stu-id="7ce06-176">tooupload a project.json file, use one of hello methods described in hello [How tooupdate function app files](functions-reference.md#fileupdate) in hello Azure Functions developer reference topic.</span></span>
3. <span data-ttu-id="7ce06-177">Hello után *project.json* fájl feltöltése úgy, hogy a következő példa a függvényben hello hasonló kimenetnek meg streaming napló:</span><span class="sxs-lookup"><span data-stu-id="7ce06-177">After hello *project.json* file is uploaded, you see output like hello following example in your function's streaming log:</span></span>

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

## <a name="environment-variables"></a><span data-ttu-id="7ce06-178">Környezeti változók</span><span class="sxs-lookup"><span data-stu-id="7ce06-178">Environment variables</span></span>
<span data-ttu-id="7ce06-179">tooget egy környezeti változó vagy egy alkalmazás beállításérték, használjon `System.Environment.GetEnvironmentVariable`, ahogy az alábbi kódpéldát hello:</span><span class="sxs-lookup"><span data-stu-id="7ce06-179">tooget an environment variable or an app setting value, use `System.Environment.GetEnvironmentVariable`, as shown in hello following code example:</span></span>

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

## <a name="reusing-csx-code"></a><span data-ttu-id="7ce06-180">Újbóli felhasználása .csx kódot</span><span class="sxs-lookup"><span data-stu-id="7ce06-180">Reusing .csx code</span></span>
<span data-ttu-id="7ce06-181">Osztályok és az egyéb megadott módszerek *.csx* -fájlok a *run.csx* fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce06-181">You can use classes and methods defined in other *.csx* files in your *run.csx* file.</span></span> <span data-ttu-id="7ce06-182">használó, toodo `#load` irányelvek a *run.csx* fájlt.</span><span class="sxs-lookup"><span data-stu-id="7ce06-182">toodo that, use `#load` directives in your *run.csx* file.</span></span> <span data-ttu-id="7ce06-183">A következő példa hello, a naplózás rutin nevű `MyLogger` a megosztott *myLogger.csx* és betöltve ebbe *run.csx* hello segítségével `#load` irányelv:</span><span class="sxs-lookup"><span data-stu-id="7ce06-183">In hello following example, a logging routine named `MyLogger` is shared in *myLogger.csx* and loaded into *run.csx* using hello `#load` directive:</span></span>

<span data-ttu-id="7ce06-184">Példa *run.csx*:</span><span class="sxs-lookup"><span data-stu-id="7ce06-184">Example *run.csx*:</span></span>

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

<span data-ttu-id="7ce06-185">Példa *mylogger.csx*:</span><span class="sxs-lookup"><span data-stu-id="7ce06-185">Example *mylogger.csx*:</span></span>

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

<span data-ttu-id="7ce06-186">Egy megosztott használatával *.csx* egy közös mintát, ha azt szeretné, hogy toostrongly a típusargumentumok egy POCO objektummal funkciók között.</span><span class="sxs-lookup"><span data-stu-id="7ce06-186">Using a shared *.csx* is a common pattern when you want toostrongly type your arguments between functions using a POCO object.</span></span> <span data-ttu-id="7ce06-187">A következő egyszerűsített példa hello, egy HTTP-eseményindítóval és várólista eseményindító megosztani egy POCO nevű objektum `Order` toostrongly típus hello rendelés adatokat:</span><span class="sxs-lookup"><span data-stu-id="7ce06-187">In hello following simplified example, an HTTP trigger and queue trigger share a POCO object named `Order` toostrongly type hello order data:</span></span>

<span data-ttu-id="7ce06-188">Példa *run.csx* HTTP-eseményindító:</span><span class="sxs-lookup"><span data-stu-id="7ce06-188">Example *run.csx* for HTTP trigger:</span></span>

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

<span data-ttu-id="7ce06-189">Példa *run.csx* várólista eseményindító:</span><span class="sxs-lookup"><span data-stu-id="7ce06-189">Example *run.csx* for queue trigger:</span></span>

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

<span data-ttu-id="7ce06-190">Példa *order.csx*:</span><span class="sxs-lookup"><span data-stu-id="7ce06-190">Example *order.csx*:</span></span>

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

<span data-ttu-id="7ce06-191">Relatív elérési út használatával hello `#load` irányelv:</span><span class="sxs-lookup"><span data-stu-id="7ce06-191">You can use a relative path with hello `#load` directive:</span></span>

* <span data-ttu-id="7ce06-192">`#load "mylogger.csx"`hello függvény mappában található fájl betöltése.</span><span class="sxs-lookup"><span data-stu-id="7ce06-192">`#load "mylogger.csx"` loads a file located in hello function folder.</span></span>
* <span data-ttu-id="7ce06-193">`#load "loadedfiles\mylogger.csx"`hello függvény mappában található a mappában található fájl betöltése.</span><span class="sxs-lookup"><span data-stu-id="7ce06-193">`#load "loadedfiles\mylogger.csx"` loads a file located in a folder in hello function folder.</span></span>
* <span data-ttu-id="7ce06-194">`#load "..\shared\mylogger.csx"`egy mappában hello azonos szinten hello függvény mappaként Ez azt jelenti, hogy a fájlt közvetlenül a *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="7ce06-194">`#load "..\shared\mylogger.csx"` loads a file located in a folder at hello same level as hello function folder, that is, directly under *wwwroot*.</span></span>

<span data-ttu-id="7ce06-195">Hello `#load` direktíva csak működik *.csx* (C#) parancsfájlok, nem pedig az *.cs* fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7ce06-195">hello `#load` directive works only with *.csx* (C# script) files, not with *.cs* files.</span></span>

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a><span data-ttu-id="7ce06-196">Imperatív kötéseken keresztül futásidejű kötés</span><span class="sxs-lookup"><span data-stu-id="7ce06-196">Binding at runtime via imperative bindings</span></span>

<span data-ttu-id="7ce06-197">C# és egyéb .NET nyelven, használhat egy [imperatív](https://en.wikipedia.org/wiki/Imperative_programming) kötés mintát, mert megakadályozását toohello [ *deklaratív* ](https://en.wikipedia.org/wiki/Declarative_programming) kötések *function.json*.</span><span class="sxs-lookup"><span data-stu-id="7ce06-197">In C# and other .NET languages, you can use an [imperative](https://en.wikipedia.org/wiki/Imperative_programming) binding pattern, as opposed toohello [*declarative*](https://en.wikipedia.org/wiki/Declarative_programming) bindings in *function.json*.</span></span> <span data-ttu-id="7ce06-198">Imperatív kötés akkor hasznos, ha a kötési paraméterekhez kell tervezési helyett futásidejű időpontban számított toobe.</span><span class="sxs-lookup"><span data-stu-id="7ce06-198">Imperative binding is useful when binding parameters need toobe computed at runtime rather than design time.</span></span> <span data-ttu-id="7ce06-199">Az ebben a mintában toosupported bemeneti kötése, és kötést a azonnali a kimenetet a funkciókódot.</span><span class="sxs-lookup"><span data-stu-id="7ce06-199">With this pattern, you can bind toosupported input and output binding on-the-fly in your function code.</span></span>

<span data-ttu-id="7ce06-200">Adja meg a következő kötés dolgozik:</span><span class="sxs-lookup"><span data-stu-id="7ce06-200">Define an imperative binding as follows:</span></span>

- <span data-ttu-id="7ce06-201">**Ne** egy bejegyzést a *function.json* számára a kívánt imperatív kötések.</span><span class="sxs-lookup"><span data-stu-id="7ce06-201">**Do not** include an entry in *function.json* for your desired imperative bindings.</span></span>
- <span data-ttu-id="7ce06-202">A bemeneti paraméter fázis [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) vagy [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).</span><span class="sxs-lookup"><span data-stu-id="7ce06-202">Pass in an input parameter [`Binder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) or [`IBinder binder`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).</span></span>
- <span data-ttu-id="7ce06-203">A következő C# mintát tooperform hello adatkötés hello használata.</span><span class="sxs-lookup"><span data-stu-id="7ce06-203">Use hello following C# pattern tooperform hello data binding.</span></span>

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

<span data-ttu-id="7ce06-204">Ha `BindingTypeAttribute` hello .NET attribútum, amely meghatározza a kötés és `T` van hello bemeneti vagy kimeneti típus a kötési típus által támogatott.</span><span class="sxs-lookup"><span data-stu-id="7ce06-204">where `BindingTypeAttribute` is hello .NET attribute that defines your binding and `T` is hello input or output type that's supported by that binding type.</span></span> <span data-ttu-id="7ce06-205">`T`nem lehet egy `out` típusú paraméter (például `out JObject`).</span><span class="sxs-lookup"><span data-stu-id="7ce06-205">`T` also cannot be an `out` parameter type (such as `out JObject`).</span></span> <span data-ttu-id="7ce06-206">Például a Mobile Apps tábla kimeneti kötése támogatja [hat kimeneti típusok](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), de csak használható [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) vagy [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)a `T`.</span><span class="sxs-lookup"><span data-stu-id="7ce06-206">For example, the Mobile Apps table output binding supports [six output types](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), but you can only use [ICollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) or [IAsyncCollector<T>](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) for `T`.</span></span>

<span data-ttu-id="7ce06-207">a következő példakód hello létrehoz egy [tárolási blob kimeneti kötése](functions-bindings-storage-blob.md#using-a-blob-output-binding) blob a futási időben megadott elérési út ezután ír egy karakterlánc toohello blob.</span><span class="sxs-lookup"><span data-stu-id="7ce06-207">hello following example code creates a [Storage blob output binding](functions-bindings-storage-blob.md#using-a-blob-output-binding) with blob path that's defined at run time, then writes a string toohello blob.</span></span>

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

<span data-ttu-id="7ce06-208">[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) hello meghatározása [tárolási blob](functions-bindings-storage-blob.md) bemeneti vagy kimeneti kötelező, és [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) támogatott kimeneti kötési típus.</span><span class="sxs-lookup"><span data-stu-id="7ce06-208">[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) defines hello [Storage blob](functions-bindings-storage-blob.md) input or output binding, and [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) is a supported output binding type.</span></span>
<span data-ttu-id="7ce06-209">Hello kód kap-e a hello alkalmazás alapértelmezés szerint hello tárolási fiók kapcsolati karakterlánc (amely `AzureWebJobsStorage`).</span><span class="sxs-lookup"><span data-stu-id="7ce06-209">As is, hello code gets hello default app setting for hello Storage account connection string (which is `AzureWebJobsStorage`).</span></span> <span data-ttu-id="7ce06-210">Megadhat egy egyéni alkalmazás-beállítás toouse hozzáadásával a [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) , és átadja hello attribútum tömb be `BindAsync<T>()`.</span><span class="sxs-lookup"><span data-stu-id="7ce06-210">You can specify a custom app setting toouse by adding the [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) and passing hello attribute array into `BindAsync<T>()`.</span></span> <span data-ttu-id="7ce06-211">Például:</span><span class="sxs-lookup"><span data-stu-id="7ce06-211">For example,</span></span>

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

<span data-ttu-id="7ce06-212">hello következő táblázatban hello .NET attribútumok minden kötés típusa és hello csomagok, amelyben vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="7ce06-212">hello following table lists hello .NET attributes for each binding type and hello packages in which they are defined.</span></span>

> [!div class="mx-codeBreakAll"]
| <span data-ttu-id="7ce06-213">Kötelező</span><span class="sxs-lookup"><span data-stu-id="7ce06-213">Binding</span></span> | <span data-ttu-id="7ce06-214">Attribútum</span><span class="sxs-lookup"><span data-stu-id="7ce06-214">Attribute</span></span> | <span data-ttu-id="7ce06-215">Hivatkozás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7ce06-215">Add reference</span></span> |
|------|------|------|
| <span data-ttu-id="7ce06-216">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="7ce06-216">Cosmos DB</span></span> | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| <span data-ttu-id="7ce06-217">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="7ce06-217">Event Hubs</span></span> | <span data-ttu-id="7ce06-218">[`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7ce06-218">[`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span></span> | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| <span data-ttu-id="7ce06-219">Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="7ce06-219">Mobile Apps</span></span> | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| <span data-ttu-id="7ce06-220">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="7ce06-220">Notification Hubs</span></span> | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| <span data-ttu-id="7ce06-221">Service Bus</span><span class="sxs-lookup"><span data-stu-id="7ce06-221">Service Bus</span></span> | <span data-ttu-id="7ce06-222">[`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7ce06-222">[`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs)</span></span> | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| <span data-ttu-id="7ce06-223">Storage üzenetsorába</span><span class="sxs-lookup"><span data-stu-id="7ce06-223">Storage queue</span></span> | <span data-ttu-id="7ce06-224">[`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7ce06-224">[`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="7ce06-225">Storage-blobba</span><span class="sxs-lookup"><span data-stu-id="7ce06-225">Storage blob</span></span> | <span data-ttu-id="7ce06-226">[`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7ce06-226">[`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="7ce06-227">Tárolási tábla</span><span class="sxs-lookup"><span data-stu-id="7ce06-227">Storage table</span></span> | <span data-ttu-id="7ce06-228">[`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span><span class="sxs-lookup"><span data-stu-id="7ce06-228">[`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)</span></span> | |
| <span data-ttu-id="7ce06-229">Twilio</span><span class="sxs-lookup"><span data-stu-id="7ce06-229">Twilio</span></span> | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a><span data-ttu-id="7ce06-230">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ce06-230">Next steps</span></span>
<span data-ttu-id="7ce06-231">További információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="7ce06-231">For more information, see hello following resources:</span></span>

* [<span data-ttu-id="7ce06-232">Azure Functions – ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="7ce06-232">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="7ce06-233">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="7ce06-233">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="7ce06-234">Az Azure Functions F # fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="7ce06-234">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="7ce06-235">Az Azure Functions NodeJS fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="7ce06-235">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="7ce06-236">Az Azure Functions eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="7ce06-236">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)

[állomás\.json]: https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json
