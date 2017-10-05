---
title: "Erőforrások telepítése az Azure Functions függvény alkalmazások automatizálása |} Microsoft Docs"
description: "Ismerje meg, hogyan hozhat létre egy Azure Resource Manager-sablon, amely a függvény alkalmazást telepíti."
services: Functions
documtationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure funkciók, a Funkciók, a kiszolgáló nélküli architektúra, a kódot, az azure erőforrás-kezelő infrastruktúra"
ms.assetid: d20743e3-aab6-442c-a836-9bcea09bfd32
ms.server: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: 15496e4ab2858b2aa319d53f1c438a259a3d5e49
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a><span data-ttu-id="85f80-104">Az Azure Functions függvény alkalmazás erőforrás-telepítés automatizálása</span><span class="sxs-lookup"><span data-stu-id="85f80-104">Automate resource deployment for your function app in Azure Functions</span></span>

<span data-ttu-id="85f80-105">Az Azure Resource Manager-sablon segítségével függvény alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="85f80-105">You can use an Azure Resource Manager template to deploy a function app.</span></span> <span data-ttu-id="85f80-106">Ez a cikk ismerteti a szükséges erőforrások és a paraméterek úgy.</span><span class="sxs-lookup"><span data-stu-id="85f80-106">This article outlines the required resources and parameters for doing so.</span></span> <span data-ttu-id="85f80-107">Előfordulhat, hogy kell telepítenie további erőforrások, attól függően, a [eseményindítók és kötések](functions-triggers-bindings.md) az függvény alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="85f80-107">You might need to deploy additional resources, depending on the [triggers and bindings](functions-triggers-bindings.md) in your function app.</span></span>

<span data-ttu-id="85f80-108">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="85f80-108">For more information about creating templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="85f80-109">A minta-sablonok lásd:</span><span class="sxs-lookup"><span data-stu-id="85f80-109">For sample templates, see:</span></span>
- <span data-ttu-id="85f80-110">[függvény app a fogyasztás terv]</span><span class="sxs-lookup"><span data-stu-id="85f80-110">[Function app on Consumption plan]</span></span>
- <span data-ttu-id="85f80-111">[függvény alkalmazást az Azure App Service-csomag]</span><span class="sxs-lookup"><span data-stu-id="85f80-111">[Function app on Azure App Service plan]</span></span>

## <a name="required-resources"></a><span data-ttu-id="85f80-112">Szükséges erőforrások</span><span class="sxs-lookup"><span data-stu-id="85f80-112">Required resources</span></span>

<span data-ttu-id="85f80-113">A függvény az alkalmazás csak ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="85f80-113">A function app requires these resources:</span></span>

* <span data-ttu-id="85f80-114">Egy [Azure Storage](../storage/index.md) fiók</span><span class="sxs-lookup"><span data-stu-id="85f80-114">An [Azure Storage](../storage/index.md) account</span></span>
* <span data-ttu-id="85f80-115">Üzemeltetési terv (fogyasztás terv vagy App Service-csomag)</span><span class="sxs-lookup"><span data-stu-id="85f80-115">A hosting plan (Consumption plan or App Service plan)</span></span>
* <span data-ttu-id="85f80-116">Egy függvény alkalmazást</span><span class="sxs-lookup"><span data-stu-id="85f80-116">A function app</span></span> 

### <a name="storage-account"></a><span data-ttu-id="85f80-117">Tárfiók</span><span class="sxs-lookup"><span data-stu-id="85f80-117">Storage account</span></span>

<span data-ttu-id="85f80-118">Egy Azure storage-fiók egy függvény app szükség.</span><span class="sxs-lookup"><span data-stu-id="85f80-118">An Azure storage account is required for a function app.</span></span> <span data-ttu-id="85f80-119">Általános célú fiók, amely támogatja a BLOB, táblák, üzenetsorok és fájlok szükséges.</span><span class="sxs-lookup"><span data-stu-id="85f80-119">You need a general purpose account that supports blobs, tables, queues, and files.</span></span> <span data-ttu-id="85f80-120">További információkért lásd: [Azure Functions tárolási fiókra vonatkozó követelmények](functions-create-function-app-portal.md#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="85f80-120">For more information, see [Azure Functions storage account requirements](functions-create-function-app-portal.md#storage-account-requirements).</span></span>

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
        "accountType": "[parameters('storageAccountType')]"
    }
}
```

<span data-ttu-id="85f80-121">Emellett a Tulajdonságok `AzureWebJobsStorage` és `AzureWebJobsDashboard` app beállítások a webhely konfigurációs kell megadni.</span><span class="sxs-lookup"><span data-stu-id="85f80-121">In addition, the properties `AzureWebJobsStorage` and `AzureWebJobsDashboard` must be specified as app settings in the site configuration.</span></span> <span data-ttu-id="85f80-122">Az Azure Functions futtatókörnyezettel használja a `AzureWebJobsStorage` kapcsolati karakterlánc belső várólisták létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="85f80-122">The Azure Functions runtime uses the `AzureWebJobsStorage` connection string to create internal queues.</span></span> <span data-ttu-id="85f80-123">A kapcsolati karakterlánc `AzureWebJobsDashboard` Azure Table storage és power naplózásakor használatos a **figyelő** lapon a portálon.</span><span class="sxs-lookup"><span data-stu-id="85f80-123">The connection string `AzureWebJobsDashboard` is used to log to Azure Table storage and power the **Monitor** tab in the portal.</span></span>

<span data-ttu-id="85f80-124">Ezek a tulajdonságok vannak megadva a `appSettings` gyűjtemény a `siteConfig` objektum:</span><span class="sxs-lookup"><span data-stu-id="85f80-124">These properties are specified in the `appSettings` collection in the `siteConfig` object:</span></span>

```json
"appSettings": [
    {
        "name": "AzureWebJobsStorage",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    },
    {
        "name": "AzureWebJobsDashboard",
        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
    }
```    

### <a name="hosting-plan"></a><span data-ttu-id="85f80-125">Üzemeltetési terv</span><span class="sxs-lookup"><span data-stu-id="85f80-125">Hosting plan</span></span>

<span data-ttu-id="85f80-126">Az üzemeltetési terv meghatározásának függ, hogy használ-e a felhasználási vagy App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="85f80-126">The definition of the hosting plan varies, depending on whether you use a Consumption or App Service plan.</span></span> <span data-ttu-id="85f80-127">Lásd: [a fogyasztás terven függvény alkalmazás telepítése](#consumption) és [függvény alkalmazás az App Service-csomag telepítése](#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="85f80-127">See [Deploy a function app on the Consumption plan](#consumption) and [Deploy a function app on the App Service plan](#app-service-plan).</span></span>

### <a name="function-app"></a><span data-ttu-id="85f80-128">Függvény alkalmazás</span><span class="sxs-lookup"><span data-stu-id="85f80-128">Function app</span></span>

<span data-ttu-id="85f80-129">A függvény app erőforrás van definiálva típusú erőforrások használatával **Microsoft.Web/Site** és fajta **functionapp**:</span><span class="sxs-lookup"><span data-stu-id="85f80-129">The function app resource is defined by using a resource of type **Microsoft.Web/Site** and kind **functionapp**:</span></span>

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ]
```

<a name="consumption"></a>

## <a name="deploy-a-function-app-on-the-consumption-plan"></a><span data-ttu-id="85f80-130">A felhasználási terven függvény alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="85f80-130">Deploy a function app on the Consumption plan</span></span>

<span data-ttu-id="85f80-131">Két különböző módban is futtathatja egy függvény alkalmazás: a fogyasztás terv és az App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="85f80-131">You can run a function app in two different modes: the Consumption plan and the App Service plan.</span></span> <span data-ttu-id="85f80-132">A felhasználási terv automatikusan osztja ki a számítási teljesítményt, a kódja fut, kimenő terhelés kezelésére, szükség szerint arányosan, és majd arányosan csökken, amikor a kód nem fut.</span><span class="sxs-lookup"><span data-stu-id="85f80-132">The Consumption plan automatically allocates compute power when your code is running, scales out as necessary to handle load, and then scales down when code is not running.</span></span> <span data-ttu-id="85f80-133">Igen nem kell a tétlen virtuális gépek után kell fizetnie, és nem kell előzetesen tartalékkapacitás.</span><span class="sxs-lookup"><span data-stu-id="85f80-133">So, you don't have to pay for idle VMs, and you don't have to reserve capacity in advance.</span></span> <span data-ttu-id="85f80-134">Üzemeltetési tervek kapcsolatos további információkért lásd: [Azure Functions használat és az App Service csomagokban](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="85f80-134">To learn more about hosting plans, see [Azure Functions Consumption and App Service plans](functions-scale.md).</span></span>

<span data-ttu-id="85f80-135">Lásd: a minta Azure Resource Manager sablon [függvény app a fogyasztás terv].</span><span class="sxs-lookup"><span data-stu-id="85f80-135">For a sample Azure Resource Manager template, see [Function app on Consumption plan].</span></span>

### <a name="create-a-consumption-plan"></a><span data-ttu-id="85f80-136">Felhasználás terv létrehozása</span><span class="sxs-lookup"><span data-stu-id="85f80-136">Create a Consumption plan</span></span>

<span data-ttu-id="85f80-137">A felhasználási terv "kiszolgálófarm" erőforrás különleges típusú.</span><span class="sxs-lookup"><span data-stu-id="85f80-137">A Consumption plan is a special type of "serverfarm" resource.</span></span> <span data-ttu-id="85f80-138">Használatával megadja a `Dynamic` értékét a `computeMode` és `sku` tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="85f80-138">You specify it by using the `Dynamic` value for the `computeMode` and `sku` properties:</span></span>

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "computeMode": "Dynamic",
        "sku": "Dynamic"
    }
}
```

### <a name="create-a-function-app"></a><span data-ttu-id="85f80-139">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="85f80-139">Create a function app</span></span>

<span data-ttu-id="85f80-140">Emellett a fogyasztás terv kell két további beállítások a webhely konfigurációs: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` és `WEBSITE_CONTENTSHARE`.</span><span class="sxs-lookup"><span data-stu-id="85f80-140">In addition, a Consumption plan requires two additional settings in the site configuration: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` and `WEBSITE_CONTENTSHARE`.</span></span> <span data-ttu-id="85f80-141">Ezek a tulajdonságok konfigurálása a tárolási fiók és a fájl elérési útja az alkalmazáskódot függvény és a konfiguráció tárolásához.</span><span class="sxs-lookup"><span data-stu-id="85f80-141">These properties configure the storage account and file path where the function app code and configuration are stored.</span></span>

```json
{
    "apiVersion": "2015-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('functionAppName')]",
    "location": "[resourceGroup().location]",
    "kind": "functionapp",            
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
    ],
    "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "siteConfig": {
            "appSettings": [
                {
                    "name": "AzureWebJobsDashboard",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "AzureWebJobsStorage",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING",
                    "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
                },
                {
                    "name": "WEBSITE_CONTENTSHARE",
                    "value": "[toLower(variables('functionAppName'))]"
                },
                {
                    "name": "FUNCTIONS_EXTENSION_VERSION",
                    "value": "~1"
                }
            ]
        }
    }
}
```                    

<a name="app-service-plan"></a> 

## <a name="deploy-a-function-app-on-the-app-service-plan"></a><span data-ttu-id="85f80-142">Egy függvény alkalmazás az App Service-csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="85f80-142">Deploy a function app on the App Service plan</span></span>

<span data-ttu-id="85f80-143">Az App Service-csomag a függvény alkalmazás fut, a Basic, Standard és Premium termékváltozat, a webalkalmazások hasonló dedikált virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="85f80-143">In the App Service plan, your function app runs on dedicated VMs on Basic, Standard, and Premium SKUs, similar to web apps.</span></span> <span data-ttu-id="85f80-144">Az App Service-csomag működésével kapcsolatos részletekért lásd: a [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="85f80-144">For details about how the App Service plan works, see the [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

<span data-ttu-id="85f80-145">Lásd: a minta Azure Resource Manager sablon [függvény alkalmazást az Azure App Service-csomag].</span><span class="sxs-lookup"><span data-stu-id="85f80-145">For a sample Azure Resource Manager template, see [Function app on Azure App Service plan].</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="85f80-146">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="85f80-146">Create an App Service plan</span></span>

```json
{
    "type": "Microsoft.Web/serverfarms",
    "apiVersion": "2015-04-01",
    "name": "[variables('hostingPlanName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "name": "[variables('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "hostingEnvironment": "",
        "numberOfWorkers": 1
    }
}
```

### <a name="create-a-function-app"></a><span data-ttu-id="85f80-147">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="85f80-147">Create a function app</span></span> 

<span data-ttu-id="85f80-148">Miután kijelölt egy beállítást, a függvény-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="85f80-148">After you've selected a scaling option, create a function app.</span></span> <span data-ttu-id="85f80-149">Az alkalmazás a tároló, amely a függvények.</span><span class="sxs-lookup"><span data-stu-id="85f80-149">The app is the container that holds all your functions.</span></span>

<span data-ttu-id="85f80-150">A függvény az alkalmazás rendelkezik sok gyermekszintű erőforrása, amely a központi telepítésre, beleértve az alkalmazásbeállítások és az adatforrás beállításokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="85f80-150">A function app has many child resources that you can use in your deployment, including app settings and source control options.</span></span> <span data-ttu-id="85f80-151">Előfordulhat, hogy meg távolítsa el a **sourcecontrols** gyermek-erőforrás és a, valamint egy másik [rendszerbe állítási beállításának](functions-continuous-deployment.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="85f80-151">You also might choose to remove the **sourcecontrols** child resource, and use a different [deployment option](functions-continuous-deployment.md) instead.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85f80-152">Az alkalmazás sikeres közzétételéhez Azure Resource Manager használatával, fontos megérteni, hogyan vannak telepítve az erőforrások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="85f80-152">To successfully deploy your application by using Azure Resource Manager, it's important to understand how resources are deployed in Azure.</span></span> <span data-ttu-id="85f80-153">A következő példában a legfelső szintű konfigurációk használatával alkalmazhatók **siteConfig**.</span><span class="sxs-lookup"><span data-stu-id="85f80-153">In the following example, top-level configurations are applied by using **siteConfig**.</span></span> <span data-ttu-id="85f80-154">Fontos állítsa be ezeket a konfigurációkat a legfelső szintű, mert ezek a funkciók futási és telepítési motor információáramlás.</span><span class="sxs-lookup"><span data-stu-id="85f80-154">It's important to set these configurations at a top level, because they convey information to the Functions runtime and deployment engine.</span></span> <span data-ttu-id="85f80-155">Legfelső szintű adatokat a gyermek előtt meg kell adni **sourcecontrols vagy webes** erőforrás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="85f80-155">Top-level information is required before the child **sourcecontrols/web** resource is applied.</span></span> <span data-ttu-id="85f80-156">Bár ezek a beállítások konfigurálására a gyermekszintű lehetséges **config/appSettings** erőforrás, a függvény app telepítenie kell bizonyos esetekben *előtt* **config/appSettings**  vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="85f80-156">Although it's possible to configure these settings in the child-level **config/appSettings** resource, in some cases your function app must be deployed *before* **config/appSettings** is applied.</span></span> <span data-ttu-id="85f80-157">Például használata esetén a funkciók [Logic Apps](../logic-apps/index.md), a függvények egy másik erőforrás függősége.</span><span class="sxs-lookup"><span data-stu-id="85f80-157">For example, when you are using functions with [Logic Apps](../logic-apps/index.md), your functions are a dependency of another resource.</span></span>

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('appName')]",
  "type": "Microsoft.Web/sites",
  "kind": "functionapp",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Web/serverfarms', parameters('appName'))]"
  ],
  "properties": {
     "serverFarmId": "[variables('appServicePlanName')]",
     "siteConfig": {
        "alwaysOn": true,
        "appSettings": [
            { "name": "FUNCTIONS_EXTENSION_VERSION", "value": "~1" },
            { "name": "Project", "value": "src" }
        ]
     }
  },
  "resources": [
     {
        "apiVersion": "2015-08-01",
        "name": "appsettings",
        "type": "config",
        "dependsOn": [
          "[resourceId('Microsoft.Web/Sites', parameters('appName'))]",
          "[resourceId('Microsoft.Web/Sites/sourcecontrols', parameters('appName'), 'web')]",
          "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
        ],
        "properties": {
          "AzureWebJobsStorage": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]",
          "AzureWebJobsDashboard": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountid'),'2015-05-01-preview').key1)]"
        }
     },
     {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/sites/', parameters('appName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('sourceCodeRepositoryURL')]",
            "branch": "[parameters('sourceCodeBranch')]",
            "IsManualIntegration": "[parameters('sourceCodeManualIntegration')]"
          }
     }
  ]
}
```
> [!TIP]
> <span data-ttu-id="85f80-158">Ez a sablon által a [projekt](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app beállítások értéket, amely beállítja a alapkönyvtárának, amelyben a funkciók telepítési motor (Kudu) megkeresi telepíthető.</span><span class="sxs-lookup"><span data-stu-id="85f80-158">This template uses the [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app settings value, which sets the base directory in which the Functions deployment engine (Kudu) looks for deployable code.</span></span> <span data-ttu-id="85f80-159">A funkciók vannak a tárházban almappájában a **src** mappát.</span><span class="sxs-lookup"><span data-stu-id="85f80-159">In our repository, our functions are in a subfolder of the **src** folder.</span></span> <span data-ttu-id="85f80-160">Igen, az előző példában azt állítsa be az alkalmazás beállítások értéket `src`.</span><span class="sxs-lookup"><span data-stu-id="85f80-160">So, in the preceding example, we set the app settings value to `src`.</span></span> <span data-ttu-id="85f80-161">Ha a funkciók a tárház gyökérkönyvtárában, vagy ha nem telepíti a verziókövetésből, eltávolíthatja az alkalmazás beállításainak érték.</span><span class="sxs-lookup"><span data-stu-id="85f80-161">If your functions are in the root of your repository, or if you are not deploying from source control, you can remove this app settings value.</span></span>

## <a name="deploy-your-template"></a><span data-ttu-id="85f80-162">A sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="85f80-162">Deploy your template</span></span>

<span data-ttu-id="85f80-163">A sablon telepítéséhez az alábbi módokon használhatja:</span><span class="sxs-lookup"><span data-stu-id="85f80-163">You can use any of the following ways to deploy your template:</span></span>

* [<span data-ttu-id="85f80-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="85f80-164">PowerShell</span></span>](../azure-resource-manager/resource-group-template-deploy.md)
* [<span data-ttu-id="85f80-165">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="85f80-165">Azure CLI</span></span>](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [<span data-ttu-id="85f80-166">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="85f80-166">Azure portal</span></span>](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [<span data-ttu-id="85f80-167">REST API</span><span class="sxs-lookup"><span data-stu-id="85f80-167">REST API</span></span>](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-to-azure-button"></a><span data-ttu-id="85f80-168">Telepítse az Azure gomb</span><span class="sxs-lookup"><span data-stu-id="85f80-168">Deploy to Azure button</span></span>

<span data-ttu-id="85f80-169">Cserélje le ```<url-encoded-path-to-azuredeploy-json>``` rendelkező egy [URL-kódolású](https://www.bing.com/search?q=url+encode) nyers elérési útja verzióját a `azuredeploy.json` fájlt a Githubon.</span><span class="sxs-lookup"><span data-stu-id="85f80-169">Replace ```<url-encoded-path-to-azuredeploy-json>``` with a [URL-encoded](https://www.bing.com/search?q=url+encode) version of the raw path of your `azuredeploy.json` file in GitHub.</span></span>

<span data-ttu-id="85f80-170">Íme egy példa a markdown használó:</span><span class="sxs-lookup"><span data-stu-id="85f80-170">Here is an example that uses markdown:</span></span>

```markdown
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

<span data-ttu-id="85f80-171">Íme egy példa a HTML formátumú:</span><span class="sxs-lookup"><span data-stu-id="85f80-171">Here is an example that uses HTML:</span></span>

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a><span data-ttu-id="85f80-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85f80-172">Next steps</span></span>

<span data-ttu-id="85f80-173">Fejleszthet, és az Azure Functions konfigurálásával kapcsolatos további tudnivalókért.</span><span class="sxs-lookup"><span data-stu-id="85f80-173">Learn more about how to develop and configure Azure Functions.</span></span>

* [<span data-ttu-id="85f80-174">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="85f80-174">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="85f80-175">Az Azure-függvény Alkalmazásbeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="85f80-175">How to configure Azure function app settings</span></span>](functions-how-to-use-azure-function-app-settings.md)
* [<span data-ttu-id="85f80-176">Az első Azure-függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="85f80-176">Create your first Azure function</span></span>](functions-create-first-azure-function.md)

<!-- LINKS -->

[függvény app a fogyasztás terv]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[függvény alkalmazást az Azure App Service-csomag]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
