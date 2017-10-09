---
title: "aaaDevelop és futtatási Azure functions helyileg |} Microsoft Docs"
description: "Ismerje meg, hogyan toocode és tesztelési Azure működik a helyi számítógépen az Azure Functions futtatása előtt."
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 07/12/2017
ms.author: glenga
ms.openlocfilehash: 342ed4d6df41a2d2df9067948e19e347bb52c844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="code-and-test-azure-functions-locally"></a><span data-ttu-id="3ee97-103">Kód és helyileg az Azure functions tesztelése</span><span class="sxs-lookup"><span data-stu-id="3ee97-103">Code and test Azure functions locally</span></span>

<span data-ttu-id="3ee97-104">Hello közben [Azure-portálon] teljes készlete eszközök fejlesztési és tesztelési Azure Functions számos fejlesztők inkább egy helyi fejlesztési felület biztosít.</span><span class="sxs-lookup"><span data-stu-id="3ee97-104">While hello [Azure portal] provides a full set of tools for developing and testing Azure Functions, many developers prefer a local development experience.</span></span> <span data-ttu-id="3ee97-105">Az Azure Functions segítségével könnyen toouse meg kedvenc kód szerkesztő és a helyi fejlesztési eszközök toodevelop, és tesztelje a funkciók a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3ee97-105">Azure Functions makes it easy toouse your favorite code editor and local development tools toodevelop and test your functions on your local computer.</span></span> <span data-ttu-id="3ee97-106">A funkciók is elindíthatja az eseményeket az Azure-ban, és a C# és JavaScript-funkcióként is debug a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3ee97-106">Your functions can trigger on events in Azure, and you can debug your C# and JavaScript functions on your local computer.</span></span> 

<span data-ttu-id="3ee97-107">A Visual Studio C# fejlesztő, az Azure Functions is [integrálható a Visual Studio 2017](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="3ee97-107">If you are a Visual Studio C# developer, Azure Functions also [integrates with Visual Studio 2017](functions-develop-vs.md).</span></span>

## <a name="install-hello-azure-functions-core-tools"></a><span data-ttu-id="3ee97-108">Hello Azure Functions Core eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="3ee97-108">Install hello Azure Functions Core Tools</span></span>

<span data-ttu-id="3ee97-109">Az Azure Functions Core eszközök futtathatja a helyi számítógépen a Windows hello Azure Functions futtatókörnyezettel helyi verziója telepítve.</span><span class="sxs-lookup"><span data-stu-id="3ee97-109">Azure Functions Core Tools is a local version of hello Azure Functions runtime that you can run on your local Windows computer.</span></span> <span data-ttu-id="3ee97-110">Nincs emulátor vagy szimulátor.</span><span class="sxs-lookup"><span data-stu-id="3ee97-110">It's not an emulator or simulator.</span></span> <span data-ttu-id="3ee97-111">Azonos futásidejű rendelkezik azt, hogy az Azure Functions powers hello</span><span class="sxs-lookup"><span data-stu-id="3ee97-111">It's hello same runtime that powers Functions in Azure.</span></span>

<span data-ttu-id="3ee97-112">Hello [Azure Functions Core eszközök] is letöltheti az npm-csomagot.</span><span class="sxs-lookup"><span data-stu-id="3ee97-112">hello [Azure Functions Core Tools] is provided as an npm package.</span></span> <span data-ttu-id="3ee97-113">Először [NodeJS telepítése](https://docs.npmjs.com/getting-started/installing-node), mely tartalmazza az npm.</span><span class="sxs-lookup"><span data-stu-id="3ee97-113">You must first [install NodeJS](https://docs.npmjs.com/getting-started/installing-node), which includes npm.</span></span>  

>[!NOTE]
><span data-ttu-id="3ee97-114">Ilyenkor a Windows rendszerű számítógépeken csak hello Azure Functions Core eszközcsomag is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="3ee97-114">At this time, hello Azure Functions Core Tools package can only be installed on Windows computers.</span></span> <span data-ttu-id="3ee97-115">Ez a korlátozás tooa átmeneti korlátozás hello funkciók fogadó miatt van.</span><span class="sxs-lookup"><span data-stu-id="3ee97-115">This restriction is due tooa temporary limitation in hello Functions host.</span></span>

<span data-ttu-id="3ee97-116">[Azure Functions Core eszközök] ad hozzá a következő parancs aliasok hello:</span><span class="sxs-lookup"><span data-stu-id="3ee97-116">[Azure Functions Core Tools] adds hello following command aliases:</span></span>
* <span data-ttu-id="3ee97-117">**FUNC**</span><span class="sxs-lookup"><span data-stu-id="3ee97-117">**func**</span></span>
* <span data-ttu-id="3ee97-118">**azfun**</span><span class="sxs-lookup"><span data-stu-id="3ee97-118">**azfun**</span></span>
* <span data-ttu-id="3ee97-119">**azurefunctions**</span><span class="sxs-lookup"><span data-stu-id="3ee97-119">**azurefunctions**</span></span>

<span data-ttu-id="3ee97-120">Ahelyett, hogy az összes alábbi alias is használható `func` hello példákban ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="3ee97-120">All of these alias can be used instead of `func` shown in hello examples in this topic.</span></span>

## <a name="create-a-local-functions-project"></a><span data-ttu-id="3ee97-121">Helyi funkciók-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ee97-121">Create a local Functions project</span></span>

<span data-ttu-id="3ee97-122">A helyi futtatás során egy funkciók projekt hello fájlok host.json és local.settings.json megegyező nevű könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="3ee97-122">When running locally, a Functions project is a directory that has hello files host.json and local.settings.json.</span></span> <span data-ttu-id="3ee97-123">Ebben a könyvtárban van hello egyenértékű, a függvény alkalmazások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3ee97-123">This directory is hello equivalent of a function app in Azure.</span></span> <span data-ttu-id="3ee97-124">toolearn hello Azure Functions gyökérmappa-szerkezetében kapcsolatos további információkért lásd: hello [Azure Functions fejlesztői útmutatója](functions-reference.md#folder-structure).</span><span class="sxs-lookup"><span data-stu-id="3ee97-124">toolearn more about hello Azure Functions folder structure, see hello [Azure Functions developers guide](functions-reference.md#folder-structure).</span></span>

<span data-ttu-id="3ee97-125">Parancsot egy parancssorba futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3ee97-125">At a command prompt, run hello following command:</span></span>

```
func init MyFunctionProj
```

<span data-ttu-id="3ee97-126">hello kimenete a következő példa hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="3ee97-126">hello output looks like hello following example:</span></span>

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

<span data-ttu-id="3ee97-127">tooopt kívül a Git-tárház, az hello kapcsolóval létrehozása `--no-source-control [-n]`.</span><span class="sxs-lookup"><span data-stu-id="3ee97-127">tooopt out of creating a Git repository, use hello option `--no-source-control [-n]`.</span></span>

<a name="local-settings"></a>

## <a name="local-settings-file"></a><span data-ttu-id="3ee97-128">Helyi fájl</span><span class="sxs-lookup"><span data-stu-id="3ee97-128">Local settings file</span></span>

<span data-ttu-id="3ee97-129">hello fájl local.settings.json Alkalmazásbeállítások, a kapcsolati karakterláncok és az Azure Functions Core eszközök beállításai tárolja.</span><span class="sxs-lookup"><span data-stu-id="3ee97-129">hello file local.settings.json stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="3ee97-130">A következő struktúra hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3ee97-130">It has hello following structure:</span></span>

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>", 
  },
  "Host": {
    "LocalHttpPort": 7071, 
    "CORS": "*" 
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```
| <span data-ttu-id="3ee97-131">Beállítás</span><span class="sxs-lookup"><span data-stu-id="3ee97-131">Setting</span></span>      | <span data-ttu-id="3ee97-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="3ee97-132">Description</span></span>                            |
| ------------ | -------------------------------------- |
| <span data-ttu-id="3ee97-133">**IsEncrypted**</span><span class="sxs-lookup"><span data-stu-id="3ee97-133">**IsEncrypted**</span></span> | <span data-ttu-id="3ee97-134">Ha értéke túl**igaz**, minden értéket a helyi számítógép kulccsal titkosított.</span><span class="sxs-lookup"><span data-stu-id="3ee97-134">When set too**true**, all values are encrypted using a local machine key.</span></span> <span data-ttu-id="3ee97-135">A használt `func settings` parancsok.</span><span class="sxs-lookup"><span data-stu-id="3ee97-135">Used with `func settings` commands.</span></span> <span data-ttu-id="3ee97-136">Alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="3ee97-136">Default value is **false**.</span></span> |
| <span data-ttu-id="3ee97-137">**Értékek**</span><span class="sxs-lookup"><span data-stu-id="3ee97-137">**Values**</span></span> | <span data-ttu-id="3ee97-138">A helyi futtatás során használt Alkalmazásbeállítások gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="3ee97-138">Collection of application settings used when running locally.</span></span> <span data-ttu-id="3ee97-139">Adja hozzá az alkalmazás beállításainak toothis objektumot.</span><span class="sxs-lookup"><span data-stu-id="3ee97-139">Add your application settings toothis object.</span></span>  |
| <span data-ttu-id="3ee97-140">**AzureWebJobsStorage**</span><span class="sxs-lookup"><span data-stu-id="3ee97-140">**AzureWebJobsStorage**</span></span> | <span data-ttu-id="3ee97-141">Készletek hello kapcsolati karakterlánc toohello belsőleg hello Azure Functions futtatókörnyezettel Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="3ee97-141">Sets hello connection string toohello Azure Storage account that is used internally by hello Azure Functions runtime.</span></span> <span data-ttu-id="3ee97-142">hello tárfiók a függvény eseményindítók támogatja.</span><span class="sxs-lookup"><span data-stu-id="3ee97-142">hello storage account supports your function's triggers.</span></span> <span data-ttu-id="3ee97-143">A tárolási fiók kapcsolat beállítását indított HTTP funkciók kivételével minden funkciók szükség.</span><span class="sxs-lookup"><span data-stu-id="3ee97-143">This storage account connection setting is required for all functions except for HTTP triggered functions.</span></span>  |
| <span data-ttu-id="3ee97-144">**AzureWebJobsDashboard**</span><span class="sxs-lookup"><span data-stu-id="3ee97-144">**AzureWebJobsDashboard**</span></span> | <span data-ttu-id="3ee97-145">Beállítja a hello kapcsolati karakterlánc toohello Azure Storage-fiók, amely használt toostore hello függvény naplókat.</span><span class="sxs-lookup"><span data-stu-id="3ee97-145">Sets hello connection string toohello Azure Storage account that is used toostore hello function logs.</span></span> <span data-ttu-id="3ee97-146">Ezt az értéket nem kötelező hello portálon elérhetővé hello naplókat.</span><span class="sxs-lookup"><span data-stu-id="3ee97-146">This optional value makes hello logs accessible in hello portal.</span></span>|
| <span data-ttu-id="3ee97-147">**Állomás**</span><span class="sxs-lookup"><span data-stu-id="3ee97-147">**Host**</span></span> | <span data-ttu-id="3ee97-148">Ebben a szakaszban beállítások testreszabása hello funkciók gazdafolyamat, a helyi futtatás során.</span><span class="sxs-lookup"><span data-stu-id="3ee97-148">Settings in this section customize hello Functions host process when running locally.</span></span> | 
| <span data-ttu-id="3ee97-149">**LocalHttpPort**</span><span class="sxs-lookup"><span data-stu-id="3ee97-149">**LocalHttpPort**</span></span> | <span data-ttu-id="3ee97-150">Készletek hello hello funkciók localhost futtatásakor használt alapértelmezett port (`func host start` és `func run`).</span><span class="sxs-lookup"><span data-stu-id="3ee97-150">Sets hello default port used when running hello local Functions host (`func host start` and `func run`).</span></span> <span data-ttu-id="3ee97-151">Hello `--port` parancssori kapcsoló elsőbbséget élvez ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="3ee97-151">hello `--port` command-line option takes precedence over this value.</span></span> |
| <span data-ttu-id="3ee97-152">**CORS**</span><span class="sxs-lookup"><span data-stu-id="3ee97-152">**CORS**</span></span> | <span data-ttu-id="3ee97-153">Meghatározza az engedélyezett hello források [eltérő eredetű erőforrások megosztása (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="3ee97-153">Defines hello origins allowed for [cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span> <span data-ttu-id="3ee97-154">Források, szóközök nélkül vesszővel tagolt lista formájában vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="3ee97-154">Origins are supplied as a comma-separated list with no spaces.</span></span> <span data-ttu-id="3ee97-155">helyettesítő karakteres érték hello (**\***) támogatott, amely lehetővé teszi a kérelmek bármely a forrásból.</span><span class="sxs-lookup"><span data-stu-id="3ee97-155">hello wildcard value (**\***) is supported, which allows requests from any origin.</span></span> |
| <span data-ttu-id="3ee97-156">**ConnectionStrings**</span><span class="sxs-lookup"><span data-stu-id="3ee97-156">**ConnectionStrings**</span></span> | <span data-ttu-id="3ee97-157">A függvények hello adatbázis-kapcsolati karakterláncok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3ee97-157">Contains hello database connection strings for your functions.</span></span> <span data-ttu-id="3ee97-158">Ez az objektum kapcsolati karakterláncokat kerülnek toohello környezet hello szolgáltató típusú **System.Data.SqlClient**.</span><span class="sxs-lookup"><span data-stu-id="3ee97-158">Connection strings in this object are added toohello environment with hello provider type of **System.Data.SqlClient**.</span></span>  | 

<span data-ttu-id="3ee97-159">A legtöbb eseményindítók és kötések rendelkezik egy **kapcsolat** tulajdonság, amely leképezhető a környezeti változó vagy alkalmazás beállításokat toohello nevét.</span><span class="sxs-lookup"><span data-stu-id="3ee97-159">Most triggers and bindings have a **Connection** property that maps toohello name of an environment variable or app setting.</span></span> <span data-ttu-id="3ee97-160">Minden kapcsolat tulajdonság local.settings.json fájlban meghatározott Alkalmazásbeállítás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3ee97-160">For each connection property, there must be app setting defined in local.settings.json file.</span></span> 

<span data-ttu-id="3ee97-161">Ezek a beállítások is elolvashatja a kódban környezeti változóként.</span><span class="sxs-lookup"><span data-stu-id="3ee97-161">These settings can also be read in your code as environment variables.</span></span> <span data-ttu-id="3ee97-162">A C#, használjon [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) vagy [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ee97-162">In C#, use [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) or [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span></span> <span data-ttu-id="3ee97-163">A JavaScript, használjon `process.env`.</span><span class="sxs-lookup"><span data-stu-id="3ee97-163">In JavaScript, use `process.env`.</span></span> <span data-ttu-id="3ee97-164">A rendszer környezeti változó megadott érvényesülnek hello local.settings.json fájl értékeit.</span><span class="sxs-lookup"><span data-stu-id="3ee97-164">Settings specified as a system environment variable take precedence over values in hello local.settings.json file.</span></span> 

<span data-ttu-id="3ee97-165">Hello local.settings.json fájl beállításai csak által használt funkciók eszközök a helyi futtatás során.</span><span class="sxs-lookup"><span data-stu-id="3ee97-165">Settings in hello local.settings.json file are only used by Functions tools when running locally.</span></span> <span data-ttu-id="3ee97-166">Alapértelmezés szerint ezek a beállítások nem települnek át automatikusan közzétett tooAzure hello projekt esetén.</span><span class="sxs-lookup"><span data-stu-id="3ee97-166">By default, these settings are not migrated automatically when hello project is published tooAzure.</span></span> <span data-ttu-id="3ee97-167">Használjon hello `--publish-local-settings` kapcsoló [közzétételekor](#publish) toomake meg arról, hogy ezek a beállítások vannak toohello függvény alkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3ee97-167">Use hello `--publish-local-settings` switch [when you publish](#publish) toomake sure these settings are added toohello function app in Azure.</span></span>

<span data-ttu-id="3ee97-168">Ha nincs érvényes tárolási kapcsolati karakterlánc beállítása a **AzureWebJobsStorage**, hello a következő hibaüzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="3ee97-168">When no valid storage connection string is set for **AzureWebJobsStorage**, hello following error message is shown:</span></span>  

><span data-ttu-id="3ee97-169">Hiányzó érték a AzureWebJobsStorage local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="3ee97-169">Missing value for AzureWebJobsStorage in local.settings.json.</span></span> <span data-ttu-id="3ee97-170">Ez azért szükséges, az összes eseményindítók HTTP eltérő.</span><span class="sxs-lookup"><span data-stu-id="3ee97-170">This is required for all triggers other than HTTP.</span></span> <span data-ttu-id="3ee97-171">Futtatása "func azure functionary fetch--Alkalmazásbeállítások <functionAppName>", vagy adjon meg egy kapcsolati karakterláncot a local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="3ee97-171">You can run 'func azure functionary fetch-app-settings <functionAppName>' or specify a connection string in local.settings.json.</span></span>
  
[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a><span data-ttu-id="3ee97-172">Alkalmazásbeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3ee97-172">Configure app settings</span></span>

<span data-ttu-id="3ee97-173">kapcsolati karakterláncok értékét tooset, tegye hello következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="3ee97-173">tooset a value for connection strings, you can do one of hello following:</span></span>
* <span data-ttu-id="3ee97-174">Adjon meg hello kapcsolati karakterláncot a [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="3ee97-174">Enter hello connection string from [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
* <span data-ttu-id="3ee97-175">Hello a következő parancsok egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="3ee97-175">Use one of hello following commands:</span></span>

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    <span data-ttu-id="3ee97-176">Mindkét parancsok, toofirst bejelentkezési tooAzure igényelnek.</span><span class="sxs-lookup"><span data-stu-id="3ee97-176">Both commands require you toofirst sign-in tooAzure.</span></span>

## <a name="create-a-function"></a><span data-ttu-id="3ee97-177">Függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ee97-177">Create a function</span></span>

<span data-ttu-id="3ee97-178">egy függvény toocreate hello a következő parancsot futtassa:</span><span class="sxs-lookup"><span data-stu-id="3ee97-178">toocreate a function, run hello following command:</span></span>

```
func new
``` 
<span data-ttu-id="3ee97-179">`func new`nem kötelező argumentum a következő hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="3ee97-179">`func new` supports hello following optional arguments:</span></span>

| <span data-ttu-id="3ee97-180">Argumentum</span><span class="sxs-lookup"><span data-stu-id="3ee97-180">Argument</span></span>     | <span data-ttu-id="3ee97-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="3ee97-181">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | <span data-ttu-id="3ee97-182">programozási nyelv, például a C#, F # vagy JavaScript hello sablont.</span><span class="sxs-lookup"><span data-stu-id="3ee97-182">hello template programming language, such as C#, F#, or JavaScript.</span></span> |
| **`--template -t`** | <span data-ttu-id="3ee97-183">hello a sablonnevet.</span><span class="sxs-lookup"><span data-stu-id="3ee97-183">hello template name.</span></span> |
| **`--name -n`** | <span data-ttu-id="3ee97-184">hello függvény neve.</span><span class="sxs-lookup"><span data-stu-id="3ee97-184">hello function name.</span></span> |

<span data-ttu-id="3ee97-185">Például toocreate egy JavaScript HTTP-eseményindítóval futtatása:</span><span class="sxs-lookup"><span data-stu-id="3ee97-185">For example, toocreate a JavaScript HTTP trigger, run:</span></span>

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

<span data-ttu-id="3ee97-186">a várólista-eseményindítóval aktivált függvény toocreate futtatása:</span><span class="sxs-lookup"><span data-stu-id="3ee97-186">toocreate a queue-triggered function, run:</span></span>

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a><span data-ttu-id="3ee97-187">Futtassa helyben a Funkciók</span><span class="sxs-lookup"><span data-stu-id="3ee97-187">Run functions locally</span></span>

<span data-ttu-id="3ee97-188">a funkciók projekt toorun hello funkciók állomás futtassa.</span><span class="sxs-lookup"><span data-stu-id="3ee97-188">toorun a Functions project, run hello Functions host.</span></span> <span data-ttu-id="3ee97-189">hello állomás lehetővé teszi, hogy az eseményindítók hello projekt összes funkciójának:</span><span class="sxs-lookup"><span data-stu-id="3ee97-189">hello host enables triggers for all functions in hello project:</span></span>

```
func host start
```

<span data-ttu-id="3ee97-190">`func host start`támogatja az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="3ee97-190">`func host start` supports hello following options:</span></span>

| <span data-ttu-id="3ee97-191">Beállítás</span><span class="sxs-lookup"><span data-stu-id="3ee97-191">Option</span></span>     | <span data-ttu-id="3ee97-192">Leírás</span><span class="sxs-lookup"><span data-stu-id="3ee97-192">Description</span></span>                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | <span data-ttu-id="3ee97-193">hello helyi port toolisten meg.</span><span class="sxs-lookup"><span data-stu-id="3ee97-193">hello local port toolisten on.</span></span> <span data-ttu-id="3ee97-194">Alapértelmezett érték: 7071.</span><span class="sxs-lookup"><span data-stu-id="3ee97-194">Default value: 7071.</span></span> |
| **`--debug <type>`** | <span data-ttu-id="3ee97-195">hello beállítások `VSCode` és `VS`.</span><span class="sxs-lookup"><span data-stu-id="3ee97-195">hello options are `VSCode` and `VS`.</span></span> |
| **`--cors`** | <span data-ttu-id="3ee97-196">A CORS források, szóközök nélkül vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="3ee97-196">A comma-separated list of CORS origins, with no spaces.</span></span> |
| **`--nodeDebugPort -n`** | <span data-ttu-id="3ee97-197">hello csomópont hibakereső toouse hello port.</span><span class="sxs-lookup"><span data-stu-id="3ee97-197">hello port for hello node debugger toouse.</span></span> <span data-ttu-id="3ee97-198">Alapértelmezett: Launch.json vagy 5858 egy értéket.</span><span class="sxs-lookup"><span data-stu-id="3ee97-198">Default: A value from launch.json or 5858.</span></span> |
| **`--debugLevel -d`** | <span data-ttu-id="3ee97-199">hello konzol nyomkövetési szint (kikapcsolt, részletes, információ, figyelmeztetés vagy hiba).</span><span class="sxs-lookup"><span data-stu-id="3ee97-199">hello console trace level (off, verbose, info, warning, or error).</span></span> <span data-ttu-id="3ee97-200">Alapértelmezett: adatait.</span><span class="sxs-lookup"><span data-stu-id="3ee97-200">Default: Info.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="3ee97-201">hello időtúllépés hello funkciók állomás elindítása, másodpercben.</span><span class="sxs-lookup"><span data-stu-id="3ee97-201">hello time out for hello Functions host t     o start, in seconds.</span></span> <span data-ttu-id="3ee97-202">Alapértelmezett: 20 másodperc.</span><span class="sxs-lookup"><span data-stu-id="3ee97-202">Default: 20 seconds.</span></span>|
| **`--useHttps`** | <span data-ttu-id="3ee97-203">Kötési toohttps://localhost: {port} helyett toohttp://localhost: {port}.</span><span class="sxs-lookup"><span data-stu-id="3ee97-203">Bind toohttps://localhost:{port} rather than toohttp://localhost:{port}.</span></span> <span data-ttu-id="3ee97-204">Ez a beállítás alapértelmezés szerint létrehoz megbízható tanúsítvány a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3ee97-204">By default, this option creates a trusted certificate on your computer.</span></span>|
| **`--pause-on-error`** | <span data-ttu-id="3ee97-205">Felfüggesztés előtt hello folyamat kilép további adatokat.</span><span class="sxs-lookup"><span data-stu-id="3ee97-205">Pause for additional input before exiting hello process.</span></span> <span data-ttu-id="3ee97-206">Akkor hasznos, ha az Azure Functions Core eszközök fókusza az integrált fejlesztési környezeti (IDE).</span><span class="sxs-lookup"><span data-stu-id="3ee97-206">Useful when launching Azure Functions Core Tools from an integrated development environment (IDE).</span></span>|

<span data-ttu-id="3ee97-207">Hello funkciók gazdagép indításakor azt hello URL-cím a HTTP-eseményindítókkal aktivált függvényeket kimenete:</span><span class="sxs-lookup"><span data-stu-id="3ee97-207">When hello Functions host starts, it outputs hello URL of HTTP-triggered functions:</span></span>

```
Found hello following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a><span data-ttu-id="3ee97-208">A Visual STUDIO Code vagy a Visual Studio hibakeresési</span><span class="sxs-lookup"><span data-stu-id="3ee97-208">Debug in VS Code or Visual Studio</span></span>

<span data-ttu-id="3ee97-209">tooattach hibakereső, átadni hello `--debug` argumentum.</span><span class="sxs-lookup"><span data-stu-id="3ee97-209">tooattach a debugger, pass hello `--debug` argument.</span></span> <span data-ttu-id="3ee97-210">JavaScript-funkcióként toodebug, a Visual Studio Code használja.</span><span class="sxs-lookup"><span data-stu-id="3ee97-210">toodebug JavaScript functions, use Visual Studio Code.</span></span> <span data-ttu-id="3ee97-211">C# funkciók a Visual Studio használata.</span><span class="sxs-lookup"><span data-stu-id="3ee97-211">For C# functions, use Visual Studio.</span></span>

<span data-ttu-id="3ee97-212">toodebug C# funkciók használata `--debug vs`.</span><span class="sxs-lookup"><span data-stu-id="3ee97-212">toodebug C# functions, use `--debug vs`.</span></span> <span data-ttu-id="3ee97-213">Is [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="3ee97-213">You can also use [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span> 

<span data-ttu-id="3ee97-214">toolaunch hello állomás, és állítsa be a JavaScript-hibakeresés, futtassa:</span><span class="sxs-lookup"><span data-stu-id="3ee97-214">toolaunch hello host and set up JavaScript debugging, run:</span></span>

```
func host start --debug vscode
```

<span data-ttu-id="3ee97-215">Ezt követően a Visual Studio Code, a hello **Debug** nézetben jelölje ki **tooAzure funkciók csatolása**.</span><span class="sxs-lookup"><span data-stu-id="3ee97-215">Then, in Visual Studio Code, in hello **Debug** view, select **Attach tooAzure Functions**.</span></span> <span data-ttu-id="3ee97-216">Töréspontokat csatolása, vizsgálja meg a változók és kód lépéseit.</span><span class="sxs-lookup"><span data-stu-id="3ee97-216">You can attach breakpoints, inspect variables, and step through code.</span></span>

![JavaScript-hibakeresés a Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-tooa-function"></a><span data-ttu-id="3ee97-218">Sikeres vizsgálati adatok tooa függvény</span><span class="sxs-lookup"><span data-stu-id="3ee97-218">Passing test data tooa function</span></span>

<span data-ttu-id="3ee97-219">Egy függvény segítségével közvetlenül is hívhat `func run <FunctionName>` , és adja meg a bemeneti adatok hello függvény.</span><span class="sxs-lookup"><span data-stu-id="3ee97-219">You can also invoke a function directly by using `func run <FunctionName>` and provide input data for hello function.</span></span> <span data-ttu-id="3ee97-220">Ez a parancs hasonló toorunning feladata hello segítségével **teszt** hello Azure-portálon lapján.</span><span class="sxs-lookup"><span data-stu-id="3ee97-220">This command is similar toorunning a function using hello **Test** tab in hello Azure portal.</span></span> <span data-ttu-id="3ee97-221">Ez a parancs hello teljes funkciók gazdagépen elindítja.</span><span class="sxs-lookup"><span data-stu-id="3ee97-221">This command launches hello entire Functions host.</span></span>

<span data-ttu-id="3ee97-222">`func run`támogatja az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="3ee97-222">`func run` supports hello following options:</span></span>

| <span data-ttu-id="3ee97-223">Beállítás</span><span class="sxs-lookup"><span data-stu-id="3ee97-223">Option</span></span>     | <span data-ttu-id="3ee97-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="3ee97-224">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | <span data-ttu-id="3ee97-225">Beágyazott tartalmat.</span><span class="sxs-lookup"><span data-stu-id="3ee97-225">Inline content.</span></span> |
| **`--debug -d`** | <span data-ttu-id="3ee97-226">A hibakereső toohello gazdafolyamatokon csatolása hello függvény futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="3ee97-226">Attach a debugger toohello host process before running hello function.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="3ee97-227">Készen áll az idő toowait (másodpercben), amíg hello helyi funkciók állomás.</span><span class="sxs-lookup"><span data-stu-id="3ee97-227">Time toowait (in seconds) until hello local Functions host is ready.</span></span>|
| **`--file -f`** | <span data-ttu-id="3ee97-228">hello fájl neve toouse tartalmat.</span><span class="sxs-lookup"><span data-stu-id="3ee97-228">hello file name toouse as content.</span></span>|
| **`--no-interactive`** | <span data-ttu-id="3ee97-229">Nem kéri a bemenetben.</span><span class="sxs-lookup"><span data-stu-id="3ee97-229">Does not prompt for input.</span></span> <span data-ttu-id="3ee97-230">Automatizálási esetekben hasznos.</span><span class="sxs-lookup"><span data-stu-id="3ee97-230">Useful for automation scenarios.</span></span>|

<span data-ttu-id="3ee97-231">Például egy HTTP-eseményindítóval aktivált függvény toocall és pass tartalomtörzs, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="3ee97-231">For example, toocall an HTTP-triggered function and pass content body, run hello following command:</span></span>

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <span data-ttu-id="3ee97-232"><a name="publish"></a>TooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="3ee97-232"><a name="publish"></a>Publish tooAzure</span></span>

<span data-ttu-id="3ee97-233">a funkciók projekt tooa függvény alkalmazások az Azure használatát hello toopublish `publish` parancs:</span><span class="sxs-lookup"><span data-stu-id="3ee97-233">toopublish a Functions project tooa function app in Azure, use hello `publish` command:</span></span>

```
func azure functionapp publish <FunctionAppName>
```

<span data-ttu-id="3ee97-234">Használhatja az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="3ee97-234">You can use hello following options:</span></span>

| <span data-ttu-id="3ee97-235">Beállítás</span><span class="sxs-lookup"><span data-stu-id="3ee97-235">Option</span></span>     | <span data-ttu-id="3ee97-236">Leírás</span><span class="sxs-lookup"><span data-stu-id="3ee97-236">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  <span data-ttu-id="3ee97-237">Közzétételi beállítások a local.settings.json tooAzure toooverwrite értesítése, ha hello beállítása már létezik.</span><span class="sxs-lookup"><span data-stu-id="3ee97-237">Publish settings in local.settings.json tooAzure, prompting toooverwrite if hello setting already exists.</span></span>|
| **`--overwrite-settings -y`** | <span data-ttu-id="3ee97-238">Együtt kell használni `-i`.</span><span class="sxs-lookup"><span data-stu-id="3ee97-238">Must be used with `-i`.</span></span> <span data-ttu-id="3ee97-239">Felülírja a helyi érték AppSettings az Azure-ban, ha különböző.</span><span class="sxs-lookup"><span data-stu-id="3ee97-239">Overwrites AppSettings in Azure with local value if different.</span></span> <span data-ttu-id="3ee97-240">Alapértelmezett érték kérése.</span><span class="sxs-lookup"><span data-stu-id="3ee97-240">Default is prompt.</span></span>|

<span data-ttu-id="3ee97-241">Hello `publish` parancs feltölt hello funkciók projektkönyvtár hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="3ee97-241">hello `publish` command uploads hello contents of hello Functions project directory.</span></span> <span data-ttu-id="3ee97-242">Ha törli a fájlokat helyileg, hello `publish` parancs nem törli azokat az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="3ee97-242">If you delete files locally, hello `publish` command does not delete them from Azure.</span></span> <span data-ttu-id="3ee97-243">Hello segítségével törölheti a fájlokat az Azure-ban [Kudu eszköz](functions-how-to-use-azure-function-app-settings.md#kudu) a hello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="3ee97-243">You can delete files in Azure by using hello [Kudu tool](functions-how-to-use-azure-function-app-settings.md#kudu) in hello [Azure portal].</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ee97-244">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3ee97-244">Next steps</span></span>

<span data-ttu-id="3ee97-245">Az Azure Functions Core Tools [nyissa meg a forrás és a Githubon található](https://github.com/azure/azure-functions-cli).</span><span class="sxs-lookup"><span data-stu-id="3ee97-245">Azure Functions Core Tools is [open source and hosted on GitHub](https://github.com/azure/azure-functions-cli).</span></span>  
<span data-ttu-id="3ee97-246">egy hiba vagy a szolgáltatás kérelem toofile [nyissa meg a GitHub probléma](https://github.com/azure/azure-functions-cli/issues).</span><span class="sxs-lookup"><span data-stu-id="3ee97-246">toofile a bug or feature request, [open a GitHub issue](https://github.com/azure/azure-functions-cli/issues).</span></span> 

<!-- LINKS -->

[Azure Functions Core eszközök]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portálon]: https://portal.azure.com 
