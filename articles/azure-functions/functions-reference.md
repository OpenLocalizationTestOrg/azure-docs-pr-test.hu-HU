---
title: "Az Azure Functions kidolgozásához |} Microsoft Docs"
description: "További tudnivalók az Azure Functions fogalmakat és módszereket is, hogy funkciók Azure, az összes programozási nyelveket és kötések kifejlesztésére lesz szükség."
services: functions
documentationcenter: na
author: christopheranderson
manager: erikre
editor: 
tags: 
keywords: "Fejlesztői útmutató, azure funkciók, Funkciók, Eseményfeldolgozási, webhookokkal, a dinamikus számítási, a kiszolgáló nélküli architektúrája"
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: chrande
ms.openlocfilehash: 879be48150cfe13e31064475aa637f13f5f5f9d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-developers-guide"></a><span data-ttu-id="9fbfa-104">Az Azure Functions fejlesztői útmutatója</span><span class="sxs-lookup"><span data-stu-id="9fbfa-104">Azure Functions developers guide</span></span>
<span data-ttu-id="9fbfa-105">Az Azure Functions adott funkciókhoz ossza meg néhány alapvető technikai kulcsfogalmak és összetevők, függetlenül a nyelvet, vagy a kötés használja.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-105">In Azure Functions, specific functions share a few core technical concepts and components, regardless of the language or binding you use.</span></span> <span data-ttu-id="9fbfa-106">Ahhoz, hogy belevágjon tanulási egy adott nyelven vagy a kötési adatait, mindenképpen olvassa végig az áttekintés, amely az összes vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-106">Before you jump into learning details specific to a given language or binding, be sure to read through this overview that applies to all of them.</span></span>

<span data-ttu-id="9fbfa-107">Ez a cikk feltételezi, hogy Ön már elolvasta a [Azure Functions áttekintése](functions-overview.md) és ismeri a [WebJobs SDK fogalmakat, például a JobHost futásidejű, eseményindítók és kötések](../app-service-web/websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="9fbfa-107">This article assumes that you've already read the [Azure Functions overview](functions-overview.md) and are familiar with [WebJobs SDK concepts such as triggers, bindings, and the JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span> <span data-ttu-id="9fbfa-108">Az Azure Functions a WebJobs SDK alapul.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-108">Azure Functions is based on the WebJobs SDK.</span></span> 

## <a name="function-code"></a><span data-ttu-id="9fbfa-109">Funkciókódot</span><span class="sxs-lookup"><span data-stu-id="9fbfa-109">Function code</span></span>
<span data-ttu-id="9fbfa-110">A *függvény* az Azure Functions elsődleges fogalom.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-110">A *function* is the primary concept in Azure Functions.</span></span> <span data-ttu-id="9fbfa-111">Írhat kódot a funkció az Ön által választott nyelven, és mentse a kódot és konfigurációs fájljait ugyanabban a mappában.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-111">You write code for a function in a language of your choice and save the code and configuration files in the same folder.</span></span> <span data-ttu-id="9fbfa-112">A konfiguráció elnevezése `function.json`, amely tartalmazza a JSON-konfigurációs adatok.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-112">The configuration is named `function.json`, which contains JSON configuration data.</span></span> <span data-ttu-id="9fbfa-113">Különböző nyelveket támogatja, és minden nincs optimalizálva az adott nyelv némileg más élményt.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-113">Various languages are supported, and each one has a slightly different experience optimized to work best for that language.</span></span> 

<span data-ttu-id="9fbfa-114">A function.json fájl határozza meg, a függvénykötés és egyéb konfigurációs beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-114">The function.json file defines the function bindings and other configuration settings.</span></span> <span data-ttu-id="9fbfa-115">A futtatókörnyezet ezt a fájlt határozza meg a figyelendő események és adja át az adatokat, és vissza adatokat, a függvény végrehajtása használja.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-115">The runtime uses this file to determine the events to monitor and how to pass data into and return data from function execution.</span></span> <span data-ttu-id="9fbfa-116">A következő egy példa function.json fájl.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-116">The following is an example function.json file.</span></span>

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

<span data-ttu-id="9fbfa-117">Állítsa be a `disabled` tulajdonságot `true` megakadályozhatja, hogy a függvény a végrehajtás alatt.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-117">Set the `disabled` property to `true` to prevent the function from being executed.</span></span>

<span data-ttu-id="9fbfa-118">A `bindings` tulajdonság értéke, ahol konfigurálhatja az eseményindítók és kötések is.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-118">The `bindings` property is where you configure both triggers and bindings.</span></span> <span data-ttu-id="9fbfa-119">Minden kötésnek osztja meg néhány általános beállítások és az egyes beállítások, amelyek bizonyos típusú kötés.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-119">Each binding shares a few common settings and some settings, which are specific to a particular type of binding.</span></span> <span data-ttu-id="9fbfa-120">Minden kötésnek szükséges a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="9fbfa-120">Every binding requires the following settings:</span></span>

| <span data-ttu-id="9fbfa-121">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9fbfa-121">Property</span></span> | <span data-ttu-id="9fbfa-122">Értékek/típusok</span><span class="sxs-lookup"><span data-stu-id="9fbfa-122">Values/Types</span></span> | <span data-ttu-id="9fbfa-123">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9fbfa-123">Comments</span></span> |
| --- | --- | --- |
| `type` |<span data-ttu-id="9fbfa-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9fbfa-124">string</span></span> |<span data-ttu-id="9fbfa-125">Kötés típusa.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-125">Binding type.</span></span> <span data-ttu-id="9fbfa-126">Például: `queueTrigger`.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-126">For example, `queueTrigger`.</span></span> |
| `direction` |<span data-ttu-id="9fbfa-127">"in" "out"</span><span class="sxs-lookup"><span data-stu-id="9fbfa-127">'in', 'out'</span></span> |<span data-ttu-id="9fbfa-128">Azt jelzi, hogy a kötés adatfogadásra a függvénynek vagy adatokat küld a függvény.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-128">Indicates whether the binding is for receiving data into the function or sending data from the function.</span></span> |
| `name` |<span data-ttu-id="9fbfa-129">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9fbfa-129">string</span></span> |<span data-ttu-id="9fbfa-130">A függvény a kötött adatok használt név.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-130">The name that is used for the bound data in the function.</span></span> <span data-ttu-id="9fbfa-131">C# ez pedig egy argumentum neve; a JavaScript esetén a kulcsot a kulcs/érték listáját.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-131">For C#, this is an argument name; for JavaScript, it's the key in a key/value list.</span></span> |

## <a name="function-app"></a><span data-ttu-id="9fbfa-132">Függvény alkalmazás</span><span class="sxs-lookup"><span data-stu-id="9fbfa-132">Function app</span></span>
<span data-ttu-id="9fbfa-133">Egy vagy több egyéni függvények felügyelete együtt, amelyet az Azure App Service egy függvény alkalmazást magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-133">A function app is comprised of one or more individual functions that are managed together by Azure App Service.</span></span> <span data-ttu-id="9fbfa-134">Összes függvény alkalmazásban funkció ossza meg az árképzési csomagot, a folyamatos üzembe helyezés és a futásidejű verzióját.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-134">All of the functions in a function app share the same pricing plan, continuous deployment and runtime version.</span></span> <span data-ttu-id="9fbfa-135">Több nyelven írt funkciók összes megoszthatja függvény ugyanahhoz az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-135">Functions written in multiple languages can all share the same function app.</span></span> <span data-ttu-id="9fbfa-136">Egy függvény app gondol rendszerezését és a funkciók együttesen kezelését is.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-136">Think of a function app as a way to organize and collectively manage your functions.</span></span> 

## <a name="runtime-script-host-and-web-host"></a><span data-ttu-id="9fbfa-137">Futásidejű (script host és webkiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="9fbfa-137">Runtime (script host and web host)</span></span>
<span data-ttu-id="9fbfa-138">A runtime vagy a parancsfájlfuttató, nem az alapul szolgáló WebJobs SDK-állomás eseményeket figyeli, összegyűjti és adatokat küld, és végső soron futtatja a kódot.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-138">The runtime, or script host, is the underlying WebJobs SDK host that listens for events, gathers and sends data, and ultimately runs your code.</span></span> 

<span data-ttu-id="9fbfa-139">HTTP-eseményindítók megkönnyítésére is van olyan webes gazdagépet, amely arra tervezték, hogy a parancsfájlfuttató éles forgatókönyvekben elé elhelyezkedik.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-139">To facilitate HTTP triggers, there is also a web host that is designed to sit in front of the script host in production scenarios.</span></span> <span data-ttu-id="9fbfa-140">Két állomás rendelkező segít elkülöníteni a parancsfájlfuttató elölről fejezze be a webkiszolgáló által kezelt forgalom.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-140">Having two hosts helps to isolate the script host from the front end traffic managed by the web host.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="9fbfa-141">Mappaszerkezet</span><span class="sxs-lookup"><span data-stu-id="9fbfa-141">Folder Structure</span></span>
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

<span data-ttu-id="9fbfa-142">Amikor létrehozása a projekt funkciók telepítéséhez függvény alkalmazásokhoz az Azure App Service-ben, a helykódot, a gyökérmappa-szerkezetében is kezelheti.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-142">When setting-up a project for deploying functions to a function app in Azure App Service, you can treat this folder structure as your site code.</span></span> <span data-ttu-id="9fbfa-143">Meglévő eszközök, például a folyamatos integrációt és telepítést, vagy egyéni telepítési parancsfájl ennek transpilation kód vagy deploy idő csomag telepítése.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-143">You can use existing tools like continuous integration and deployment, or custom deployment scripts for doing deploy time package installation or code transpilation.</span></span>

> [!NOTE]
> <span data-ttu-id="9fbfa-144">Ügyeljen arra, hogy központi telepítése a `host.json` fájlt, és mappák közvetlenül működéséhez a `wwwroot` mappát.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-144">Make sure to deploy your `host.json` file and function folders directly to the `wwwroot` folder.</span></span> <span data-ttu-id="9fbfa-145">Nem tartalmaznak a `wwwroot` mappába a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-145">Do not include the `wwwroot` folder in your deployments.</span></span> <span data-ttu-id="9fbfa-146">Ellenkező esetben, végül `wwwroot\wwwroot` mappák.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-146">Otherwise, you end up with `wwwroot\wwwroot` folders.</span></span> 
> 
> 

## <span data-ttu-id="9fbfa-147"><a id="fileupdate"></a>Függvények app fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="9fbfa-147"><a id="fileupdate"></a> How to update function app files</span></span>
<span data-ttu-id="9fbfa-148">A függvény szerkesztő beépítve az Azure-portál lehetővé teszi, hogy frissítse a *function.json* fájl- és a kódfájl függvény esetében.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-148">The function editor built into the Azure portal lets you update the *function.json* file and the code file for a function.</span></span> <span data-ttu-id="9fbfa-149">Töltse fel, vagy más, mint frissítését *package.json* vagy *project.json* vagy függőségek, akkor használjon más telepítési módszert.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-149">To upload or update other files such as *package.json* or *project.json* or dependencies, you have to use other deployment methods.</span></span>

<span data-ttu-id="9fbfa-150">Függvény alkalmazások beépített App Service, így minden a [szokásos webes alkalmazásokra mutató elérhető telepítési lehetőségeket](../app-service-web/web-sites-deploy.md) függvény alkalmazások esetében is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-150">Function apps are built on App Service, so all the [deployment options available to standard web apps](../app-service-web/web-sites-deploy.md) are also available for function apps.</span></span> <span data-ttu-id="9fbfa-151">Az alábbiakban néhány módszer feltöltése vagy függvény alkalmazást frissítőfájlok használható.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-151">Here are some methods you can use to upload or update function app files.</span></span> 

#### <a name="to-use-app-service-editor"></a><span data-ttu-id="9fbfa-152">App Service-szerkesztő segítségével</span><span class="sxs-lookup"><span data-stu-id="9fbfa-152">To use App Service Editor</span></span>
1. <span data-ttu-id="9fbfa-153">Az Azure Functions portálon kattintson **Alkalmazásbeállítások működéséhez**.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-153">In the Azure Functions portal, click **Function app settings**.</span></span>
2. <span data-ttu-id="9fbfa-154">Az a **speciális beállítások** kattintson **az App Service-beállítások**.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-154">In the **Advanced Settings** section, click **Go to App Service Settings**.</span></span>
3. <span data-ttu-id="9fbfa-155">Kattintson a **App Service-szerkesztő** alkalmazás menü NAV alatt **FEJLESZTŐESZKÖZÖK**.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-155">Click **App Service Editor** in App Menu Nav under **DEVELOPMENT TOOLS**.</span></span>
4. <span data-ttu-id="9fbfa-156">Kattintson a **Ugrás**.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-156">click **Go**.</span></span>
   
   <span data-ttu-id="9fbfa-157">App Service-szerkesztő betöltése után megjelenik a *host.json* fájl- és függvény mappáit *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-157">After App Service Editor loads, you'll see the *host.json* file and function folders under *wwwroot*.</span></span> 
5. <span data-ttu-id="9fbfa-158">Nyissa meg a fájlok szerkeszthetők, vagy húzza és eltávolítása a következő fájlok feltöltése a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-158">Open files to edit them, or drag and drop from your development machine to upload files.</span></span>

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a><span data-ttu-id="9fbfa-159">A függvény app SCM (Kudu) végpont használata</span><span class="sxs-lookup"><span data-stu-id="9fbfa-159">To use the function app's SCM (Kudu) endpoint</span></span>
1. <span data-ttu-id="9fbfa-160">Nyissa meg: `https://<function_app_name>.scm.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-160">Navigate to: `https://<function_app_name>.scm.azurewebsites.net`.</span></span>
2. <span data-ttu-id="9fbfa-161">Kattintson a **konzol Debug > CMD**.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-161">Click **Debug Console > CMD**.</span></span>
3. <span data-ttu-id="9fbfa-162">Navigáljon a `D:\home\site\wwwroot\` frissítése *host.json* vagy `D:\home\site\wwwroot\<function_name>` egy függvény fájlok frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-162">Navigate to `D:\home\site\wwwroot\` to update *host.json* or `D:\home\site\wwwroot\<function_name>` to update a function's files.</span></span>
4. <span data-ttu-id="9fbfa-163">Fogd és vidd egy fájlt szeretne feltölteni a fájlt rácsban a megfelelő mappába.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-163">Drag-and-drop a file you want to upload into the appropriate folder in the file grid.</span></span> <span data-ttu-id="9fbfa-164">Nincsenek a két területen a fájl rácsban, ha egy fájl elvetné.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-164">There are two areas in the file grid where you can drop a file.</span></span> <span data-ttu-id="9fbfa-165">A *.zip* fájlok, megjelenik egy címkével ellátott "húzzon ide, és csomagolja ki."</span><span class="sxs-lookup"><span data-stu-id="9fbfa-165">For *.zip* files, a box appears with the label "Drag here to upload and unzip."</span></span> <span data-ttu-id="9fbfa-166">Minden olyan fájltípus esetében dobja el a fájl rácsban, de a "csomagolja ki" mezőben kívül.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-166">For other file types, drop in the file grid but outside the "unzip" box.</span></span>

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --DonnaM -->

#### <a name="to-use-continuous-deployment"></a><span data-ttu-id="9fbfa-167">Folyamatos üzembe helyezés használata</span><span class="sxs-lookup"><span data-stu-id="9fbfa-167">To use continuous deployment</span></span>
<span data-ttu-id="9fbfa-168">Kövesse az utasításokat a következő témakör [folyamatos üzembe helyezés az Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="9fbfa-168">Follow the instructions in the topic [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span>

## <a name="parallel-execution"></a><span data-ttu-id="9fbfa-169">Párhuzamos végrehajtás</span><span class="sxs-lookup"><span data-stu-id="9fbfa-169">Parallel execution</span></span>
<span data-ttu-id="9fbfa-170">Több eseményindító események bekövetkezésekor gyorsabb, mint az egyszálas függvény futtatókörnyezettel is dolgozza fel őket, a futtatókörnyezet alkalmazhatja a függvény párhuzamosan több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-170">When multiple triggering events occur faster than a single-threaded function runtime can process them, the runtime may invoke the function multiple times in parallel.</span></span>  <span data-ttu-id="9fbfa-171">Ha egy függvény alkalmazás használja a [üzemeltetési terv fogyasztás](functions-scale.md#how-the-consumption-plan-works), a függvény alkalmazás automatikusan sikerült kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-171">If a function app is using the [Consumption hosting plan](functions-scale.md#how-the-consumption-plan-works), the function app could scale out automatically.</span></span>  <span data-ttu-id="9fbfa-172">Függvény alkalmazás egyes példányainak hogy az alkalmazás fut-e a felhasználásra vonatkozó üzemeltetési terv vagy szokványos [az alkalmazásszolgáltatási csomag üzemeltetési](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), előfordulhat, hogy feldolgozni egyidejű függvény meghívásához párhuzamosan több szálat használ.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-172">Each instance of the function app, whether the app runs on the Consumption hosting plan or a regular [App Service hosting plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), might process concurrent function invocations in parallel using multiple threads.</span></span>  <span data-ttu-id="9fbfa-173">Maximális száma párhuzamos függvény meghívásához minden függvény alkalmazáspéldány használt eseményindítót, valamint egyéb funkciók, a függvény alkalmazásban által használt erőforrások típusú függ.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-173">The maximum number of concurrent function invocations in each function app instance varies based on the type of trigger being used as well as the resources used by other functions within the function app.</span></span>

## <a name="functions-runtime-versioning"></a><span data-ttu-id="9fbfa-174">Funkciók futásidejű versioning</span><span class="sxs-lookup"><span data-stu-id="9fbfa-174">Functions runtime versioning</span></span>

<span data-ttu-id="9fbfa-175">Konfigurálhatja a funkciók futásidejű használatával verzióját a `FUNCTIONS_EXTENSION_VERSION` Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-175">You can configure the version of the Functions runtime using the `FUNCTIONS_EXTENSION_VERSION` app setting.</span></span> <span data-ttu-id="9fbfa-176">Például a "~ 1" érték azt jelzi, hogy a függvény App 1 fog használni a fő verziószáma.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-176">For example, the value "~1" indicates that your Function App will use 1 as its major version.</span></span> <span data-ttu-id="9fbfa-177">Függvény alkalmazások verzióra, hogy minden új kisebb vannak.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-177">Function Apps are upgraded to each new minor version as they are released.</span></span> <span data-ttu-id="9fbfa-178">A függvény az alkalmazáshoz, a pontos verziójának megtekintéséhez a **beállítások** fülre az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-178">You can view the exact version of your Function App in the **Settings** tab in the Azure Portal.</span></span>

## <a name="repositories"></a><span data-ttu-id="9fbfa-179">Adattárak</span><span class="sxs-lookup"><span data-stu-id="9fbfa-179">Repositories</span></span>
<span data-ttu-id="9fbfa-180">A kód az Azure Functions nyílt forráskódú, és a GitHub-adattárak tárolja:</span><span class="sxs-lookup"><span data-stu-id="9fbfa-180">The code for Azure Functions is open source and stored in GitHub repositories:</span></span>

* [<span data-ttu-id="9fbfa-181">Az Azure Functions futtatókörnyezettel</span><span class="sxs-lookup"><span data-stu-id="9fbfa-181">Azure Functions runtime</span></span>](https://github.com/Azure/azure-webjobs-sdk-script/)
* [<span data-ttu-id="9fbfa-182">Az Azure Functions portálra</span><span class="sxs-lookup"><span data-stu-id="9fbfa-182">Azure Functions portal</span></span>](https://github.com/projectkudu/AzureFunctionsPortal)
* [<span data-ttu-id="9fbfa-183">Az Azure Functions sablonokkal</span><span class="sxs-lookup"><span data-stu-id="9fbfa-183">Azure Functions templates</span></span>](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [<span data-ttu-id="9fbfa-184">Az Azure WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="9fbfa-184">Azure WebJobs SDK</span></span>](https://github.com/Azure/azure-webjobs-sdk/)
* [<span data-ttu-id="9fbfa-185">Azure WebJobs SDK-bővítmények</span><span class="sxs-lookup"><span data-stu-id="9fbfa-185">Azure WebJobs SDK Extensions</span></span>](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a><span data-ttu-id="9fbfa-186">Kötések</span><span class="sxs-lookup"><span data-stu-id="9fbfa-186">Bindings</span></span>
<span data-ttu-id="9fbfa-187">Ez az összes támogatott kötések tábla.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-187">Here is a table of all supported bindings.</span></span>

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a><span data-ttu-id="9fbfa-188">Jelentéskészítési problémák</span><span class="sxs-lookup"><span data-stu-id="9fbfa-188">Reporting Issues</span></span>
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a><span data-ttu-id="9fbfa-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9fbfa-189">Next steps</span></span>
<span data-ttu-id="9fbfa-190">További információkért lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="9fbfa-190">For more information, see the following resources:</span></span>

* [<span data-ttu-id="9fbfa-191">Azure Functions – ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="9fbfa-191">Best Practices for Azure Functions</span></span>](functions-best-practices.md)
* [<span data-ttu-id="9fbfa-192">Az Azure Functions C# fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="9fbfa-192">Azure Functions C# developer reference</span></span>](functions-reference-csharp.md)
* [<span data-ttu-id="9fbfa-193">Az Azure Functions F # fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="9fbfa-193">Azure Functions F# developer reference</span></span>](functions-reference-fsharp.md)
* [<span data-ttu-id="9fbfa-194">Az Azure Functions NodeJS fejlesztői leírás</span><span class="sxs-lookup"><span data-stu-id="9fbfa-194">Azure Functions NodeJS developer reference</span></span>](functions-reference-node.md)
* [<span data-ttu-id="9fbfa-195">Az Azure Functions eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="9fbfa-195">Azure Functions triggers and bindings</span></span>](functions-triggers-bindings.md)
* <span data-ttu-id="9fbfa-196">[Az Azure Functions: Az út](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) az Azure App Service csapatának blogjában.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-196">[Azure Functions: The Journey](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) on the Azure App Service team blog.</span></span> <span data-ttu-id="9fbfa-197">Hogyan jött létre az Azure Functions előzményeit.</span><span class="sxs-lookup"><span data-stu-id="9fbfa-197">A history of how Azure Functions was developed.</span></span>

