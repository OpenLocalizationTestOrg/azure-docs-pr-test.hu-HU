---
title: "Fejlesztés és helyileg történő futtatása az Azure functions |} Microsoft Docs"
description: "Megtudhatja, hogyan kód és a helyi számítógépen az Azure functions tesztelése az Azure Functions futtatása előtt."
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
ms.openlocfilehash: bbe03973dbd7c70463caa6d1a45efae2ec722c05
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="code-and-test-azure-functions-locally"></a><span data-ttu-id="d7e8a-103">Kód és helyileg az Azure functions tesztelése</span><span class="sxs-lookup"><span data-stu-id="d7e8a-103">Code and test Azure functions locally</span></span>

<span data-ttu-id="d7e8a-104">Amíg a [Azure-portálon] teljes készlete eszközök fejlesztési és tesztelési Azure Functions számos fejlesztők inkább egy helyi fejlesztési felület biztosít.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-104">While the [Azure portal] provides a full set of tools for developing and testing Azure Functions, many developers prefer a local development experience.</span></span> <span data-ttu-id="d7e8a-105">Az Azure Functions megkönnyíti, hogy a kedvenc kód szerkesztése és a helyi fejlesztői eszközök segítségével történő fejlesztéséhez és teszteléséhez a funkciók a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-105">Azure Functions makes it easy to use your favorite code editor and local development tools to develop and test your functions on your local computer.</span></span> <span data-ttu-id="d7e8a-106">A funkciók is elindíthatja az eseményeket az Azure-ban, és a C# és JavaScript-funkcióként is debug a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-106">Your functions can trigger on events in Azure, and you can debug your C# and JavaScript functions on your local computer.</span></span> 

<span data-ttu-id="d7e8a-107">A Visual Studio C# fejlesztő, az Azure Functions is [integrálható a Visual Studio 2017](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-107">If you are a Visual Studio C# developer, Azure Functions also [integrates with Visual Studio 2017](functions-develop-vs.md).</span></span>

## <a name="install-the-azure-functions-core-tools"></a><span data-ttu-id="d7e8a-108">Az Azure Functions Core eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="d7e8a-108">Install the Azure Functions Core Tools</span></span>

<span data-ttu-id="d7e8a-109">Az Azure Functions Core eszközök, amelyek a helyi Windows-számítógépen futtathatja az Azure Functions futtatókörnyezettel helyi verziója telepítve.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-109">Azure Functions Core Tools is a local version of the Azure Functions runtime that you can run on your local Windows computer.</span></span> <span data-ttu-id="d7e8a-110">Nincs emulátor vagy szimulátor.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-110">It's not an emulator or simulator.</span></span> <span data-ttu-id="d7e8a-111">Az azonos futásidejű powers működik az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-111">It's the same runtime that powers Functions in Azure.</span></span>

<span data-ttu-id="d7e8a-112">A [Azure Functions Core eszközök] is letöltheti az npm-csomagot.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-112">The [Azure Functions Core Tools] is provided as an npm package.</span></span> <span data-ttu-id="d7e8a-113">Először [NodeJS telepítése](https://docs.npmjs.com/getting-started/installing-node), mely tartalmazza az npm.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-113">You must first [install NodeJS](https://docs.npmjs.com/getting-started/installing-node), which includes npm.</span></span>  

>[!NOTE]
><span data-ttu-id="d7e8a-114">Ilyenkor a Windows rendszerű számítógépeken csak az Azure Functions Core eszközcsomag is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-114">At this time, the Azure Functions Core Tools package can only be installed on Windows computers.</span></span> <span data-ttu-id="d7e8a-115">Ez a korlátozás az az oka egy átmeneti korlátozás a funkciók fogadó.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-115">This restriction is due to a temporary limitation in the Functions host.</span></span>

<span data-ttu-id="d7e8a-116">[Azure Functions Core eszközök] ad hozzá a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-116">[Azure Functions Core Tools] adds the following command aliases:</span></span>
* <span data-ttu-id="d7e8a-117">**FUNC**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-117">**func**</span></span>
* <span data-ttu-id="d7e8a-118">**azfun**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-118">**azfun**</span></span>
* <span data-ttu-id="d7e8a-119">**azurefunctions**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-119">**azurefunctions**</span></span>

<span data-ttu-id="d7e8a-120">Ahelyett, hogy az összes alábbi alias is használható `func` a példákban ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-120">All of these alias can be used instead of `func` shown in the examples in this topic.</span></span>

## <a name="create-a-local-functions-project"></a><span data-ttu-id="d7e8a-121">Helyi funkciók-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7e8a-121">Create a local Functions project</span></span>

<span data-ttu-id="d7e8a-122">Helyben fut, a funkciók projekt esetén a fájlok host.json és local.settings.json megegyező nevű könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-122">When running locally, a Functions project is a directory that has the files host.json and local.settings.json.</span></span> <span data-ttu-id="d7e8a-123">Ez a könyvtár megegyezik a függvény alkalmazások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-123">This directory is the equivalent of a function app in Azure.</span></span> <span data-ttu-id="d7e8a-124">Az Azure Functions mappaszerkezet kapcsolatos további tudnivalókért tekintse meg a [Azure Functions fejlesztői útmutatója](functions-reference.md#folder-structure).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-124">To learn more about the Azure Functions folder structure, see the [Azure Functions developers guide](functions-reference.md#folder-structure).</span></span>

<span data-ttu-id="d7e8a-125">A parancssorban futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-125">At a command prompt, run the following command:</span></span>

```
func init MyFunctionProj
```

<span data-ttu-id="d7e8a-126">A kimenet a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-126">The output looks like the following example:</span></span>

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

<span data-ttu-id="d7e8a-127">Kikapcsolja a Git-tárház létrehozása, használja a kapcsolót `--no-source-control [-n]`.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-127">To opt out of creating a Git repository, use the option `--no-source-control [-n]`.</span></span>

<a name="local-settings"></a>

## <a name="local-settings-file"></a><span data-ttu-id="d7e8a-128">Helyi fájl</span><span class="sxs-lookup"><span data-stu-id="d7e8a-128">Local settings file</span></span>

<span data-ttu-id="d7e8a-129">A fájl local.settings.json Alkalmazásbeállítások, a kapcsolati karakterláncok és az Azure Functions Core eszközök beállításai tárolja.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-129">The file local.settings.json stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="d7e8a-130">Az alábbi szerkezettel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-130">It has the following structure:</span></span>

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
| <span data-ttu-id="d7e8a-131">Beállítás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-131">Setting</span></span>      | <span data-ttu-id="d7e8a-132">Leírás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-132">Description</span></span>                            |
| ------------ | -------------------------------------- |
| <span data-ttu-id="d7e8a-133">**IsEncrypted**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-133">**IsEncrypted**</span></span> | <span data-ttu-id="d7e8a-134">Ha beállítása **igaz**, minden értéket a helyi számítógép kulccsal titkosított.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-134">When set to **true**, all values are encrypted using a local machine key.</span></span> <span data-ttu-id="d7e8a-135">A használt `func settings` parancsok.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-135">Used with `func settings` commands.</span></span> <span data-ttu-id="d7e8a-136">Alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-136">Default value is **false**.</span></span> |
| <span data-ttu-id="d7e8a-137">**Értékek**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-137">**Values**</span></span> | <span data-ttu-id="d7e8a-138">A helyi futtatás során használt Alkalmazásbeállítások gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-138">Collection of application settings used when running locally.</span></span> <span data-ttu-id="d7e8a-139">Ez az objektum hozzáadása az alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-139">Add your application settings to this object.</span></span>  |
| <span data-ttu-id="d7e8a-140">**AzureWebJobsStorage**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-140">**AzureWebJobsStorage**</span></span> | <span data-ttu-id="d7e8a-141">A kapcsolati karakterlánc beállítása az Azure Storage-fiók, amely az Azure Functions futtatókörnyezettel általi belső használatra szolgál.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-141">Sets the connection string to the Azure Storage account that is used internally by the Azure Functions runtime.</span></span> <span data-ttu-id="d7e8a-142">A tárfiók a függvény eseményindítók támogatja.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-142">The storage account supports your function's triggers.</span></span> <span data-ttu-id="d7e8a-143">A tárolási fiók kapcsolat beállítását indított HTTP funkciók kivételével minden funkciók szükség.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-143">This storage account connection setting is required for all functions except for HTTP triggered functions.</span></span>  |
| <span data-ttu-id="d7e8a-144">**AzureWebJobsDashboard**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-144">**AzureWebJobsDashboard**</span></span> | <span data-ttu-id="d7e8a-145">A kapcsolati karakterlánc beállítása az Azure Storage-fiók, amely a függvény naplók tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-145">Sets the connection string to the Azure Storage account that is used to store the function logs.</span></span> <span data-ttu-id="d7e8a-146">Ezt az értéket nem kötelező elérhetővé válnak a naplók a portálon.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-146">This optional value makes the logs accessible in the portal.</span></span>|
| <span data-ttu-id="d7e8a-147">**Állomás**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-147">**Host**</span></span> | <span data-ttu-id="d7e8a-148">Ebben a szakaszban beállítások testreszabása a funkciók gazdafolyamat, a helyi futtatás során.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-148">Settings in this section customize the Functions host process when running locally.</span></span> | 
| <span data-ttu-id="d7e8a-149">**LocalHttpPort**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-149">**LocalHttpPort**</span></span> | <span data-ttu-id="d7e8a-150">Beállítja azt a portot használja a helyi funkciók állomás fut (`func host start` és `func run`).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-150">Sets the default port used when running the local Functions host (`func host start` and `func run`).</span></span> <span data-ttu-id="d7e8a-151">A `--port` parancssori kapcsoló elsőbbséget élvez ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-151">The `--port` command-line option takes precedence over this value.</span></span> |
| <span data-ttu-id="d7e8a-152">**CORS**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-152">**CORS**</span></span> | <span data-ttu-id="d7e8a-153">Meghatározza az engedélyezett eredeteket [eltérő eredetű erőforrások megosztása (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-153">Defines the origins allowed for [cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span> <span data-ttu-id="d7e8a-154">Források, szóközök nélkül vesszővel tagolt lista formájában vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-154">Origins are supplied as a comma-separated list with no spaces.</span></span> <span data-ttu-id="d7e8a-155">A helyettesítő karakteres érték (**\***) támogatott, amely lehetővé teszi a kérelmek bármely a forrásból.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-155">The wildcard value (**\***) is supported, which allows requests from any origin.</span></span> |
| <span data-ttu-id="d7e8a-156">**ConnectionStrings**</span><span class="sxs-lookup"><span data-stu-id="d7e8a-156">**ConnectionStrings**</span></span> | <span data-ttu-id="d7e8a-157">Az adatbázis-kapcsolati karakterláncok a függvényeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-157">Contains the database connection strings for your functions.</span></span> <span data-ttu-id="d7e8a-158">Ez az objektum kapcsolati karakterláncokat hozzáadódnak a szolgáltató típusát a környezet **System.Data.SqlClient**.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-158">Connection strings in this object are added to the environment with the provider type of **System.Data.SqlClient**.</span></span>  | 

<span data-ttu-id="d7e8a-159">A legtöbb eseményindítók és kötések rendelkezik egy **kapcsolat** tulajdonság, amely leképezhető egy környezeti változó vagy alkalmazás beállítás nevét.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-159">Most triggers and bindings have a **Connection** property that maps to the name of an environment variable or app setting.</span></span> <span data-ttu-id="d7e8a-160">Minden kapcsolat tulajdonság local.settings.json fájlban meghatározott Alkalmazásbeállítás kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-160">For each connection property, there must be app setting defined in local.settings.json file.</span></span> 

<span data-ttu-id="d7e8a-161">Ezek a beállítások is elolvashatja a kódban környezeti változóként.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-161">These settings can also be read in your code as environment variables.</span></span> <span data-ttu-id="d7e8a-162">A C#, használjon [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) vagy [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-162">In C#, use [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) or [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx).</span></span> <span data-ttu-id="d7e8a-163">A JavaScript, használjon `process.env`.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-163">In JavaScript, use `process.env`.</span></span> <span data-ttu-id="d7e8a-164">A rendszer környezeti változó megadott érvényesülnek a local.settings.json fájl értékeit.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-164">Settings specified as a system environment variable take precedence over values in the local.settings.json file.</span></span> 

<span data-ttu-id="d7e8a-165">A local.settings.json fájl csak által használt funkciók eszközök a helyi futtatás során.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-165">Settings in the local.settings.json file are only used by Functions tools when running locally.</span></span> <span data-ttu-id="d7e8a-166">Alapértelmezés szerint ezek a beállítások nem települnek át automatikusan a projektet az Azure-ba való közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-166">By default, these settings are not migrated automatically when the project is published to Azure.</span></span> <span data-ttu-id="d7e8a-167">Használja a `--publish-local-settings` kapcsoló [közzétételekor](#publish) való győződjön meg arról, hogy ezek a beállítások hozzáadódnak a függvény alkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-167">Use the `--publish-local-settings` switch [when you publish](#publish) to make sure these settings are added to the function app in Azure.</span></span>

<span data-ttu-id="d7e8a-168">Ha nincs érvényes tárolási kapcsolati karakterlánc beállítása a **AzureWebJobsStorage**, a következő hibaüzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-168">When no valid storage connection string is set for **AzureWebJobsStorage**, the following error message is shown:</span></span>  

><span data-ttu-id="d7e8a-169">Hiányzó érték a AzureWebJobsStorage local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-169">Missing value for AzureWebJobsStorage in local.settings.json.</span></span> <span data-ttu-id="d7e8a-170">Ez azért szükséges, az összes eseményindítók HTTP eltérő.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-170">This is required for all triggers other than HTTP.</span></span> <span data-ttu-id="d7e8a-171">Futtatása "func azure functionary fetch--Alkalmazásbeállítások <functionAppName>", vagy adjon meg egy kapcsolati karakterláncot a local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-171">You can run 'func azure functionary fetch-app-settings <functionAppName>' or specify a connection string in local.settings.json.</span></span>
  
[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a><span data-ttu-id="d7e8a-172">Alkalmazásbeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d7e8a-172">Configure app settings</span></span>

<span data-ttu-id="d7e8a-173">Kapcsolati karakterláncok érték beállításához tegye a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-173">To set a value for connection strings, you can do one of the following:</span></span>
* <span data-ttu-id="d7e8a-174">Adja meg a kapcsolati karakterláncnak a következőről [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-174">Enter the connection string from [Azure Storage Explorer](http://storageexplorer.com/).</span></span>
* <span data-ttu-id="d7e8a-175">Használja a következő parancsok egyikét:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-175">Use one of the following commands:</span></span>

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure functionapp storage fetch-connection-string <StorageAccountName>
    ```
    <span data-ttu-id="d7e8a-176">Mindkét parancsok használatba történő első bejelentkezés az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-176">Both commands require you to first sign-in to Azure.</span></span>

## <a name="create-a-function"></a><span data-ttu-id="d7e8a-177">Függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7e8a-177">Create a function</span></span>

<span data-ttu-id="d7e8a-178">A függvény létrehozásához futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-178">To create a function, run the following command:</span></span>

```
func new
``` 
<span data-ttu-id="d7e8a-179">`func new`a következő nem kötelező argumentum használatát is támogatja:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-179">`func new` supports the following optional arguments:</span></span>

| <span data-ttu-id="d7e8a-180">Argumentum</span><span class="sxs-lookup"><span data-stu-id="d7e8a-180">Argument</span></span>     | <span data-ttu-id="d7e8a-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-181">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | <span data-ttu-id="d7e8a-182">A sablon programozási nyelv, például a C#, F # vagy JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-182">The template programming language, such as C#, F#, or JavaScript.</span></span> |
| **`--template -t`** | <span data-ttu-id="d7e8a-183">A sablonnevet.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-183">The template name.</span></span> |
| **`--name -n`** | <span data-ttu-id="d7e8a-184">A függvény nevét.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-184">The function name.</span></span> |

<span data-ttu-id="d7e8a-185">Például a JavaScript HTTP-eseményindítóval létrehozásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-185">For example, to create a JavaScript HTTP trigger, run:</span></span>

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

<span data-ttu-id="d7e8a-186">A várólista-eseményindítóval aktivált függvény létrehozásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-186">To create a queue-triggered function, run:</span></span>

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```

## <a name="run-functions-locally"></a><span data-ttu-id="d7e8a-187">Futtassa helyben a Funkciók</span><span class="sxs-lookup"><span data-stu-id="d7e8a-187">Run functions locally</span></span>

<span data-ttu-id="d7e8a-188">A funkciók projekt futtatni, futtassa a funkciók állomás.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-188">To run a Functions project, run the Functions host.</span></span> <span data-ttu-id="d7e8a-189">A gazdagép lehetővé teszi, hogy a projekt összes funkciójának eseményindítók:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-189">The host enables triggers for all functions in the project:</span></span>

```
func host start
```

<span data-ttu-id="d7e8a-190">`func host start`támogatja a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-190">`func host start` supports the following options:</span></span>

| <span data-ttu-id="d7e8a-191">Beállítás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-191">Option</span></span>     | <span data-ttu-id="d7e8a-192">Leírás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-192">Description</span></span>                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | <span data-ttu-id="d7e8a-193">A helyi portot.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-193">The local port to listen on.</span></span> <span data-ttu-id="d7e8a-194">Alapértelmezett érték: 7071.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-194">Default value: 7071.</span></span> |
| **`--debug <type>`** | <span data-ttu-id="d7e8a-195">A beállítások `VSCode` és `VS`.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-195">The options are `VSCode` and `VS`.</span></span> |
| **`--cors`** | <span data-ttu-id="d7e8a-196">A CORS források, szóközök nélkül vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-196">A comma-separated list of CORS origins, with no spaces.</span></span> |
| **`--nodeDebugPort -n`** | <span data-ttu-id="d7e8a-197">A csomópont hibakereső használandó portot.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-197">The port for the node debugger to use.</span></span> <span data-ttu-id="d7e8a-198">Alapértelmezett: Launch.json vagy 5858 egy értéket.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-198">Default: A value from launch.json or 5858.</span></span> |
| **`--debugLevel -d`** | <span data-ttu-id="d7e8a-199">A konzol nyomkövetési szint (kikapcsolt, részletes, információ, figyelmeztetés vagy hiba).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-199">The console trace level (off, verbose, info, warning, or error).</span></span> <span data-ttu-id="d7e8a-200">Alapértelmezett: adatait.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-200">Default: Info.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="d7e8a-201">Az időkorlát a funkciók állomás elindítása, másodpercben.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-201">The time out for the Functions host t     o start, in seconds.</span></span> <span data-ttu-id="d7e8a-202">Alapértelmezett: 20 másodperc.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-202">Default: 20 seconds.</span></span>|
| **`--useHttps`** | <span data-ttu-id="d7e8a-203">Https://localhost köthető: {port} helyett a http://localhost: {port}.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-203">Bind to https://localhost:{port} rather than to http://localhost:{port}.</span></span> <span data-ttu-id="d7e8a-204">Ez a beállítás alapértelmezés szerint létrehoz megbízható tanúsítvány a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-204">By default, this option creates a trusted certificate on your computer.</span></span>|
| **`--pause-on-error`** | <span data-ttu-id="d7e8a-205">A folyamat leállítása előtt szüneteltetése további adatokat.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-205">Pause for additional input before exiting the process.</span></span> <span data-ttu-id="d7e8a-206">Akkor hasznos, ha az Azure Functions Core eszközök fókusza az integrált fejlesztési környezeti (IDE).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-206">Useful when launching Azure Functions Core Tools from an integrated development environment (IDE).</span></span>|

<span data-ttu-id="d7e8a-207">A funkciók gazdagép indításakor azt az URL-cím a HTTP-eseményindítókkal aktivált függvényeket kimenete:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-207">When the Functions host starts, it outputs the URL of HTTP-triggered functions:</span></span>

```
Found the following functions:
Host.Functions.MyHttpTrigger

ob host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a><span data-ttu-id="d7e8a-208">A Visual STUDIO Code vagy a Visual Studio hibakeresési</span><span class="sxs-lookup"><span data-stu-id="d7e8a-208">Debug in VS Code or Visual Studio</span></span>

<span data-ttu-id="d7e8a-209">A hibakereső csatolásához át a `--debug` argumentum.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-209">To attach a debugger, pass the `--debug` argument.</span></span> <span data-ttu-id="d7e8a-210">JavaScript-funkcióként hibakeresési, használja a Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-210">To debug JavaScript functions, use Visual Studio Code.</span></span> <span data-ttu-id="d7e8a-211">C# funkciók a Visual Studio használata.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-211">For C# functions, use Visual Studio.</span></span>

<span data-ttu-id="d7e8a-212">Hibakeresési C# funkciók, használja a `--debug vs`.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-212">To debug C# functions, use `--debug vs`.</span></span> <span data-ttu-id="d7e8a-213">Is [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-213">You can also use [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span> 

<span data-ttu-id="d7e8a-214">Indítsa el a gazdagépen, és állítsa be a JavaScript-hibakeresés, futtassa:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-214">To launch the host and set up JavaScript debugging, run:</span></span>

```
func host start --debug vscode
```

<span data-ttu-id="d7e8a-215">Ezt követően a Visual Studio Code, az a **Debug** nézetben jelölje ki **csatlakoztatása az Azure Functions**.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-215">Then, in Visual Studio Code, in the **Debug** view, select **Attach to Azure Functions**.</span></span> <span data-ttu-id="d7e8a-216">Töréspontokat csatolása, vizsgálja meg a változók és kód lépéseit.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-216">You can attach breakpoints, inspect variables, and step through code.</span></span>

![JavaScript-hibakeresés a Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

### <a name="passing-test-data-to-a-function"></a><span data-ttu-id="d7e8a-218">Egy függvény sikeres Tesztadatok</span><span class="sxs-lookup"><span data-stu-id="d7e8a-218">Passing test data to a function</span></span>

<span data-ttu-id="d7e8a-219">Egy függvény segítségével közvetlenül is hívhat `func run <FunctionName>` , és adjon meg a függvény a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-219">You can also invoke a function directly by using `func run <FunctionName>` and provide input data for the function.</span></span> <span data-ttu-id="d7e8a-220">Ez a parancs hasonlít fut, a függvény használatával a **teszt** fülre az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-220">This command is similar to running a function using the **Test** tab in the Azure portal.</span></span> <span data-ttu-id="d7e8a-221">Ez a parancs a teljes funkciók gazdagépen elindítja.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-221">This command launches the entire Functions host.</span></span>

<span data-ttu-id="d7e8a-222">`func run`támogatja a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-222">`func run` supports the following options:</span></span>

| <span data-ttu-id="d7e8a-223">Beállítás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-223">Option</span></span>     | <span data-ttu-id="d7e8a-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-224">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | <span data-ttu-id="d7e8a-225">Beágyazott tartalmat.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-225">Inline content.</span></span> |
| **`--debug -d`** | <span data-ttu-id="d7e8a-226">A hibakereső csatolása a gazdafolyamat függvény futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-226">Attach a debugger to the host process before running the function.</span></span>|
| **`--timeout -t`** | <span data-ttu-id="d7e8a-227">Idő (másodpercben) várja meg, míg a helyi funkciók gazdagépen elkészült.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-227">Time to wait (in seconds) until the local Functions host is ready.</span></span>|
| **`--file -f`** | <span data-ttu-id="d7e8a-228">A tartalom használandó fájl neve.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-228">The file name to use as content.</span></span>|
| **`--no-interactive`** | <span data-ttu-id="d7e8a-229">Nem kéri a bemenetben.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-229">Does not prompt for input.</span></span> <span data-ttu-id="d7e8a-230">Automatizálási esetekben hasznos.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-230">Useful for automation scenarios.</span></span>|

<span data-ttu-id="d7e8a-231">Például egy HTTP-eseményindítóval aktivált függvény, és adja át a tartalomtörzs, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-231">For example, to call an HTTP-triggered function and pass content body, run the following command:</span></span>

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <span data-ttu-id="d7e8a-232"><a name="publish"></a>Közzététel az Azure platformon</span><span class="sxs-lookup"><span data-stu-id="d7e8a-232"><a name="publish"></a>Publish to Azure</span></span>

<span data-ttu-id="d7e8a-233">A funkciók projekt közzététele függvény alkalmazásokhoz az Azure-ban, használja a `publish` parancs:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-233">To publish a Functions project to a function app in Azure, use the `publish` command:</span></span>

```
func azure functionapp publish <FunctionAppName>
```

<span data-ttu-id="d7e8a-234">A következő beállításokat is használhatja:</span><span class="sxs-lookup"><span data-stu-id="d7e8a-234">You can use the following options:</span></span>

| <span data-ttu-id="d7e8a-235">Beállítás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-235">Option</span></span>     | <span data-ttu-id="d7e8a-236">Leírás</span><span class="sxs-lookup"><span data-stu-id="d7e8a-236">Description</span></span>                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  <span data-ttu-id="d7e8a-237">Közzétételi beállítások a local.settings.json az Azure-ba, arra kéri a írhatja felül, ha a beállítás már létezik.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-237">Publish settings in local.settings.json to Azure, prompting to overwrite if the setting already exists.</span></span>|
| **`--overwrite-settings -y`** | <span data-ttu-id="d7e8a-238">Együtt kell használni `-i`.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-238">Must be used with `-i`.</span></span> <span data-ttu-id="d7e8a-239">Felülírja a helyi érték AppSettings az Azure-ban, ha különböző.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-239">Overwrites AppSettings in Azure with local value if different.</span></span> <span data-ttu-id="d7e8a-240">Alapértelmezett érték kérése.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-240">Default is prompt.</span></span>|

<span data-ttu-id="d7e8a-241">A `publish` parancs feltölti a funkciók projekt könyvtár tartalmát.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-241">The `publish` command uploads the contents of the Functions project directory.</span></span> <span data-ttu-id="d7e8a-242">Ha törli a fájlokat helyileg, a `publish` parancs nem törli azokat az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="d7e8a-242">If you delete files locally, the `publish` command does not delete them from Azure.</span></span> <span data-ttu-id="d7e8a-243">Használatával törölheti a fájlokat az Azure-ban a [Kudu eszköz](functions-how-to-use-azure-function-app-settings.md#kudu) a a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="d7e8a-243">You can delete files in Azure by using the [Kudu tool](functions-how-to-use-azure-function-app-settings.md#kudu) in the [Azure portal].</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7e8a-244">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7e8a-244">Next steps</span></span>

<span data-ttu-id="d7e8a-245">Az Azure Functions Core Tools [nyissa meg a forrás és a Githubon található](https://github.com/azure/azure-functions-cli).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-245">Azure Functions Core Tools is [open source and hosted on GitHub](https://github.com/azure/azure-functions-cli).</span></span>  
<span data-ttu-id="d7e8a-246">A következő fájl egy hiba vagy a szolgáltatás kérelem [nyissa meg a GitHub probléma](https://github.com/azure/azure-functions-cli/issues).</span><span class="sxs-lookup"><span data-stu-id="d7e8a-246">To file a bug or feature request, [open a GitHub issue](https://github.com/azure/azure-functions-cli/issues).</span></span> 

<!-- LINKS -->

[Azure Functions Core eszközök]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portálon]: https://portal.azure.com 
