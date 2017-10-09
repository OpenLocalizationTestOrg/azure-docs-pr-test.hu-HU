---
title: "az Azure Functions függvény alkalmazások aaaAutomate erőforrások telepítése |} Microsoft Docs"
description: "Megtudhatja, hogyan toobuild egy Azure Resource Manager-sablon, amely a függvény alkalmazást telepíti."
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
ms.openlocfilehash: b0df0d4ef9fe93213f7b1cb1d1e6b4e14f8b3a30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-resource-deployment-for-your-function-app-in-azure-functions"></a><span data-ttu-id="96f2b-104">Az Azure Functions függvény alkalmazás erőforrás-telepítés automatizálása</span><span class="sxs-lookup"><span data-stu-id="96f2b-104">Automate resource deployment for your function app in Azure Functions</span></span>

<span data-ttu-id="96f2b-105">Az Azure Resource Manager sablon toodeploy egy függvény alkalmazást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="96f2b-105">You can use an Azure Resource Manager template toodeploy a function app.</span></span> <span data-ttu-id="96f2b-106">Ez a cikk ismerteti a hello szükséges erőforrások és a paraméterek úgy.</span><span class="sxs-lookup"><span data-stu-id="96f2b-106">This article outlines hello required resources and parameters for doing so.</span></span> <span data-ttu-id="96f2b-107">Szükség lehet további források toodeploy, attól függően, hogy hello [eseményindítók és kötések](functions-triggers-bindings.md) az függvény alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="96f2b-107">You might need toodeploy additional resources, depending on hello [triggers and bindings](functions-triggers-bindings.md) in your function app.</span></span>

<span data-ttu-id="96f2b-108">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="96f2b-108">For more information about creating templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="96f2b-109">A minta-sablonok lásd:</span><span class="sxs-lookup"><span data-stu-id="96f2b-109">For sample templates, see:</span></span>
- <span data-ttu-id="96f2b-110">[függvény app a fogyasztás terv]</span><span class="sxs-lookup"><span data-stu-id="96f2b-110">[Function app on Consumption plan]</span></span>
- <span data-ttu-id="96f2b-111">[függvény alkalmazást az Azure App Service-csomag]</span><span class="sxs-lookup"><span data-stu-id="96f2b-111">[Function app on Azure App Service plan]</span></span>

## <a name="required-resources"></a><span data-ttu-id="96f2b-112">Szükséges erőforrások</span><span class="sxs-lookup"><span data-stu-id="96f2b-112">Required resources</span></span>

<span data-ttu-id="96f2b-113">A függvény az alkalmazás csak ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="96f2b-113">A function app requires these resources:</span></span>

* <span data-ttu-id="96f2b-114">Egy [Azure Storage](../storage/index.md) fiók</span><span class="sxs-lookup"><span data-stu-id="96f2b-114">An [Azure Storage](../storage/index.md) account</span></span>
* <span data-ttu-id="96f2b-115">Üzemeltetési terv (fogyasztás terv vagy App Service-csomag)</span><span class="sxs-lookup"><span data-stu-id="96f2b-115">A hosting plan (Consumption plan or App Service plan)</span></span>
* <span data-ttu-id="96f2b-116">Egy függvény alkalmazást</span><span class="sxs-lookup"><span data-stu-id="96f2b-116">A function app</span></span> 

### <a name="storage-account"></a><span data-ttu-id="96f2b-117">Tárfiók</span><span class="sxs-lookup"><span data-stu-id="96f2b-117">Storage account</span></span>

<span data-ttu-id="96f2b-118">Egy Azure storage-fiók egy függvény app szükség.</span><span class="sxs-lookup"><span data-stu-id="96f2b-118">An Azure storage account is required for a function app.</span></span> <span data-ttu-id="96f2b-119">Általános célú fiók, amely támogatja a BLOB, táblák, üzenetsorok és fájlok szükséges.</span><span class="sxs-lookup"><span data-stu-id="96f2b-119">You need a general purpose account that supports blobs, tables, queues, and files.</span></span> <span data-ttu-id="96f2b-120">További információkért lásd: [Azure Functions tárolási fiókra vonatkozó követelmények](functions-create-function-app-portal.md#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="96f2b-120">For more information, see [Azure Functions storage account requirements](functions-create-function-app-portal.md#storage-account-requirements).</span></span>

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

<span data-ttu-id="96f2b-121">Emellett a Tulajdonságok hello `AzureWebJobsStorage` és `AzureWebJobsDashboard` Alkalmazásbeállítások hello hely konfigurációban kell megadni.</span><span class="sxs-lookup"><span data-stu-id="96f2b-121">In addition, hello properties `AzureWebJobsStorage` and `AzureWebJobsDashboard` must be specified as app settings in hello site configuration.</span></span> <span data-ttu-id="96f2b-122">hello Azure Functions futtatókörnyezete használja hello `AzureWebJobsStorage` kapcsolati karakterlánc toocreate belső várólisták.</span><span class="sxs-lookup"><span data-stu-id="96f2b-122">hello Azure Functions runtime uses hello `AzureWebJobsStorage` connection string toocreate internal queues.</span></span> <span data-ttu-id="96f2b-123">kapcsolati karakterlánc hello `AzureWebJobsDashboard` használt toolog tooAzure Table storage és a tápkábelek hello van **figyelő** hello portálon lapon.</span><span class="sxs-lookup"><span data-stu-id="96f2b-123">hello connection string `AzureWebJobsDashboard` is used toolog tooAzure Table storage and power hello **Monitor** tab in hello portal.</span></span>

<span data-ttu-id="96f2b-124">Ezeket a tulajdonságokat meg van határozva a hello `appSettings` hello gyűjtemény `siteConfig` objektum:</span><span class="sxs-lookup"><span data-stu-id="96f2b-124">These properties are specified in hello `appSettings` collection in hello `siteConfig` object:</span></span>

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

### <a name="hosting-plan"></a><span data-ttu-id="96f2b-125">Üzemeltetési terv</span><span class="sxs-lookup"><span data-stu-id="96f2b-125">Hosting plan</span></span>

<span data-ttu-id="96f2b-126">hello üzemeltetési terv meghatározásának hello függ, hogy használ-e a felhasználási vagy App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="96f2b-126">hello definition of hello hosting plan varies, depending on whether you use a Consumption or App Service plan.</span></span> <span data-ttu-id="96f2b-127">Lásd: [hello fogyasztás terven függvény alkalmazás telepítése](#consumption) és [függvény alkalmazás az App Service-csomag hello telepítése](#app-service-plan).</span><span class="sxs-lookup"><span data-stu-id="96f2b-127">See [Deploy a function app on hello Consumption plan](#consumption) and [Deploy a function app on hello App Service plan](#app-service-plan).</span></span>

### <a name="function-app"></a><span data-ttu-id="96f2b-128">Függvény alkalmazás</span><span class="sxs-lookup"><span data-stu-id="96f2b-128">Function app</span></span>

<span data-ttu-id="96f2b-129">hello függvény app erőforrás van definiálva típusú erőforrások használatával **Microsoft.Web/Site** és fajta **functionapp**:</span><span class="sxs-lookup"><span data-stu-id="96f2b-129">hello function app resource is defined by using a resource of type **Microsoft.Web/Site** and kind **functionapp**:</span></span>

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

## <a name="deploy-a-function-app-on-hello-consumption-plan"></a><span data-ttu-id="96f2b-130">A hello fogyasztás terv függvény alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="96f2b-130">Deploy a function app on hello Consumption plan</span></span>

<span data-ttu-id="96f2b-131">Egy függvény alkalmazás két különböző módban is futtathatja: hello felhasználás a terv és az App Service-csomag hello.</span><span class="sxs-lookup"><span data-stu-id="96f2b-131">You can run a function app in two different modes: hello Consumption plan and hello App Service plan.</span></span> <span data-ttu-id="96f2b-132">a kód fut-e, méretezi kimenő szükséges toohandle terhelés, és majd arányosan csökken, amikor a kód nem fut a hello fogyasztás terv automatikusan számítási teljesítményt foglal le.</span><span class="sxs-lookup"><span data-stu-id="96f2b-132">hello Consumption plan automatically allocates compute power when your code is running, scales out as necessary toohandle load, and then scales down when code is not running.</span></span> <span data-ttu-id="96f2b-133">Igen toopay tétlen virtuális gépek nem rendelkezik, és tooreserve kapacitás nincs előre.</span><span class="sxs-lookup"><span data-stu-id="96f2b-133">So, you don't have toopay for idle VMs, and you don't have tooreserve capacity in advance.</span></span> <span data-ttu-id="96f2b-134">További információ az üzemeltető terveket, toolearn lásd: [Azure Functions használat és az App Service csomagokban](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="96f2b-134">toolearn more about hosting plans, see [Azure Functions Consumption and App Service plans](functions-scale.md).</span></span>

<span data-ttu-id="96f2b-135">Lásd: a minta Azure Resource Manager sablon [függvény app a fogyasztás terv].</span><span class="sxs-lookup"><span data-stu-id="96f2b-135">For a sample Azure Resource Manager template, see [Function app on Consumption plan].</span></span>

### <a name="create-a-consumption-plan"></a><span data-ttu-id="96f2b-136">Felhasználás terv létrehozása</span><span class="sxs-lookup"><span data-stu-id="96f2b-136">Create a Consumption plan</span></span>

<span data-ttu-id="96f2b-137">A felhasználási terv "kiszolgálófarm" erőforrás különleges típusú.</span><span class="sxs-lookup"><span data-stu-id="96f2b-137">A Consumption plan is a special type of "serverfarm" resource.</span></span> <span data-ttu-id="96f2b-138">Megadja azt a hello `Dynamic` hello értéke `computeMode` és `sku` tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="96f2b-138">You specify it by using hello `Dynamic` value for hello `computeMode` and `sku` properties:</span></span>

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

### <a name="create-a-function-app"></a><span data-ttu-id="96f2b-139">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="96f2b-139">Create a function app</span></span>

<span data-ttu-id="96f2b-140">Emellett a fogyasztás terv kell hello Helykonfiguráció két további beállítások: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` és `WEBSITE_CONTENTSHARE`.</span><span class="sxs-lookup"><span data-stu-id="96f2b-140">In addition, a Consumption plan requires two additional settings in hello site configuration: `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` and `WEBSITE_CONTENTSHARE`.</span></span> <span data-ttu-id="96f2b-141">Ezek a tulajdonságok konfigurálása hello tárolási fiók és a fájlelérési út hello függvény alkalmazáskódot és konfigurációs tároló.</span><span class="sxs-lookup"><span data-stu-id="96f2b-141">These properties configure hello storage account and file path where hello function app code and configuration are stored.</span></span>

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

## <a name="deploy-a-function-app-on-hello-app-service-plan"></a><span data-ttu-id="96f2b-142">Az App Service-csomag hello függvény alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="96f2b-142">Deploy a function app on hello App Service plan</span></span>

<span data-ttu-id="96f2b-143">Az App Service-csomag hello a függvény alkalmazás fut, a Basic, Standard és Premium termékváltozat hasonló tooweb alkalmazásokra dedikált virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="96f2b-143">In hello App Service plan, your function app runs on dedicated VMs on Basic, Standard, and Premium SKUs, similar tooweb apps.</span></span> <span data-ttu-id="96f2b-144">App Service-csomag hello működésével kapcsolatos részletekért lásd: hello [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="96f2b-144">For details about how hello App Service plan works, see hello [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span> 

<span data-ttu-id="96f2b-145">Lásd: a minta Azure Resource Manager sablon [függvény alkalmazást az Azure App Service-csomag].</span><span class="sxs-lookup"><span data-stu-id="96f2b-145">For a sample Azure Resource Manager template, see [Function app on Azure App Service plan].</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="96f2b-146">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="96f2b-146">Create an App Service plan</span></span>

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

### <a name="create-a-function-app"></a><span data-ttu-id="96f2b-147">Függvényalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="96f2b-147">Create a function app</span></span> 

<span data-ttu-id="96f2b-148">Miután kijelölt egy beállítást, a függvény-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="96f2b-148">After you've selected a scaling option, create a function app.</span></span> <span data-ttu-id="96f2b-149">hello app hello tároló, amely a függvények.</span><span class="sxs-lookup"><span data-stu-id="96f2b-149">hello app is hello container that holds all your functions.</span></span>

<span data-ttu-id="96f2b-150">A függvény az alkalmazás rendelkezik sok gyermekszintű erőforrása, amely a központi telepítésre, beleértve az alkalmazásbeállítások és az adatforrás beállításokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="96f2b-150">A function app has many child resources that you can use in your deployment, including app settings and source control options.</span></span> <span data-ttu-id="96f2b-151">Akkor is előfordulhat, hogy válasszon tooremove hello **sourcecontrols** gyermek-erőforrás és a, valamint egy másik [rendszerbe állítási beállításának](functions-continuous-deployment.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="96f2b-151">You also might choose tooremove hello **sourcecontrols** child resource, and use a different [deployment option](functions-continuous-deployment.md) instead.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="96f2b-152">toosuccessfully az alkalmazás telepítése Azure Resource Manager használatával, a fontos toounderstand hogyan vannak telepítve az erőforrások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="96f2b-152">toosuccessfully deploy your application by using Azure Resource Manager, it's important toounderstand how resources are deployed in Azure.</span></span> <span data-ttu-id="96f2b-153">A következő példa hello, a legfelső szintű konfigurációk használatával alkalmazhatók **siteConfig**.</span><span class="sxs-lookup"><span data-stu-id="96f2b-153">In hello following example, top-level configurations are applied by using **siteConfig**.</span></span> <span data-ttu-id="96f2b-154">Fontos tooset ezeket a konfigurációkat a felső szinten, mert azok átadja toohello funkciók futási és telepítési motor információkat is.</span><span class="sxs-lookup"><span data-stu-id="96f2b-154">It's important tooset these configurations at a top level, because they convey information toohello Functions runtime and deployment engine.</span></span> <span data-ttu-id="96f2b-155">Legfelső szintű adatokat hello gyermek előtt meg kell adni **sourcecontrols vagy webes** erőforrás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="96f2b-155">Top-level information is required before hello child **sourcecontrols/web** resource is applied.</span></span> <span data-ttu-id="96f2b-156">Bár lehetséges tooconfigure ezeket a beállításokat a hello gyermekszintű **config/appSettings** erőforrás, a függvény app telepítenie kell bizonyos esetekben *előtt* **config/appSettings**  vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="96f2b-156">Although it's possible tooconfigure these settings in hello child-level **config/appSettings** resource, in some cases your function app must be deployed *before* **config/appSettings** is applied.</span></span> <span data-ttu-id="96f2b-157">Például használata esetén a funkciók [Logic Apps](../logic-apps/index.md), a függvények egy másik erőforrás függősége.</span><span class="sxs-lookup"><span data-stu-id="96f2b-157">For example, when you are using functions with [Logic Apps](../logic-apps/index.md), your functions are a dependency of another resource.</span></span>

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
> <span data-ttu-id="96f2b-158">Ez a sablon által hello [projekt](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app beállítások értéket, amely beállítja a hello alapkönyvtárának mely hello a funkciók telepítési motor (Kudu) megkeresi telepíthető.</span><span class="sxs-lookup"><span data-stu-id="96f2b-158">This template uses hello [Project](https://github.com/projectkudu/kudu/wiki/Customizing-deployments#using-app-settings-instead-of-a-deployment-file) app settings value, which sets hello base directory in which hello Functions deployment engine (Kudu) looks for deployable code.</span></span> <span data-ttu-id="96f2b-159">A funkciók vannak a tárházban hello almappájában **src** mappát.</span><span class="sxs-lookup"><span data-stu-id="96f2b-159">In our repository, our functions are in a subfolder of hello **src** folder.</span></span> <span data-ttu-id="96f2b-160">Igen, a fenti példa hello, hivatott hello app beállítások érték túl`src`.</span><span class="sxs-lookup"><span data-stu-id="96f2b-160">So, in hello preceding example, we set hello app settings value too`src`.</span></span> <span data-ttu-id="96f2b-161">Ha a funkciók hello a tárház gyökérkönyvtárában, vagy ha nem telepíti a verziókövetésből, eltávolíthatja az alkalmazás beállításainak érték.</span><span class="sxs-lookup"><span data-stu-id="96f2b-161">If your functions are in hello root of your repository, or if you are not deploying from source control, you can remove this app settings value.</span></span>

## <a name="deploy-your-template"></a><span data-ttu-id="96f2b-162">A sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="96f2b-162">Deploy your template</span></span>

<span data-ttu-id="96f2b-163">Használhatja a következő módokon toodeploy hello bármelyikét a sablont:</span><span class="sxs-lookup"><span data-stu-id="96f2b-163">You can use any of hello following ways toodeploy your template:</span></span>

* [<span data-ttu-id="96f2b-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="96f2b-164">PowerShell</span></span>](../azure-resource-manager/resource-group-template-deploy.md)
* [<span data-ttu-id="96f2b-165">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="96f2b-165">Azure CLI</span></span>](../azure-resource-manager/resource-group-template-deploy-cli.md)
* [<span data-ttu-id="96f2b-166">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="96f2b-166">Azure portal</span></span>](../azure-resource-manager/resource-group-template-deploy-portal.md)
* [<span data-ttu-id="96f2b-167">REST API</span><span class="sxs-lookup"><span data-stu-id="96f2b-167">REST API</span></span>](../azure-resource-manager/resource-group-template-deploy-rest.md)

### <a name="deploy-tooazure-button"></a><span data-ttu-id="96f2b-168">TooAzure gomb telepítése</span><span class="sxs-lookup"><span data-stu-id="96f2b-168">Deploy tooAzure button</span></span>

<span data-ttu-id="96f2b-169">Cserélje le ```<url-encoded-path-to-azuredeploy-json>``` rendelkező egy [URL-kódolású](https://www.bing.com/search?q=url+encode) hello nyers elérési útja verzióját a `azuredeploy.json` fájlt a Githubon.</span><span class="sxs-lookup"><span data-stu-id="96f2b-169">Replace ```<url-encoded-path-to-azuredeploy-json>``` with a [URL-encoded](https://www.bing.com/search?q=url+encode) version of hello raw path of your `azuredeploy.json` file in GitHub.</span></span>

<span data-ttu-id="96f2b-170">Íme egy példa a markdown használó:</span><span class="sxs-lookup"><span data-stu-id="96f2b-170">Here is an example that uses markdown:</span></span>

```markdown
[![Deploy tooAzure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>)
```

<span data-ttu-id="96f2b-171">Íme egy példa a HTML formátumú:</span><span class="sxs-lookup"><span data-stu-id="96f2b-171">Here is an example that uses HTML:</span></span>

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<url-encoded-path-to-azuredeploy-json>" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"></a>
```

## <a name="next-steps"></a><span data-ttu-id="96f2b-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96f2b-172">Next steps</span></span>

<span data-ttu-id="96f2b-173">További tudnivalók toodevelop és az Azure Functions konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="96f2b-173">Learn more about how toodevelop and configure Azure Functions.</span></span>

* [<span data-ttu-id="96f2b-174">Az Azure Functions fejlesztői segédanyagai</span><span class="sxs-lookup"><span data-stu-id="96f2b-174">Azure Functions developer reference</span></span>](functions-reference.md)
* [<span data-ttu-id="96f2b-175">Hogyan tooconfigure Azure működni Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="96f2b-175">How tooconfigure Azure function app settings</span></span>](functions-how-to-use-azure-function-app-settings.md)
* [<span data-ttu-id="96f2b-176">Az első Azure-függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="96f2b-176">Create your first Azure function</span></span>](functions-create-first-azure-function.md)

<!-- LINKS -->

[függvény app a fogyasztás terv]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dynamic/azuredeploy.json
[függvény alkalmazást az Azure App Service-csomag]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-function-app-create-dedicated/azuredeploy.json
