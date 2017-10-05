---
title: "Redis gyorsítótár Webalkalmazásnál kiépítése"
description: "Webes alkalmazás a Redis Cache telepítéséhez használható Azure Resource Manager-sablon."
services: app-service
documentationcenter: 
author: steved0x
manager: erickson-doug
editor: 
ms.assetid: 6e99c71f-ef8e-4570-a307-e4c059e60c35
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: sdanie
ms.openlocfilehash: 810c1cedd4fe0bd6ecdf9bd32dfb241f5f345300
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a><span data-ttu-id="91224-103">Hozzon létre egy webalkalmazást és a Redis Cache-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="91224-103">Create a Web App plus Redis Cache using a template</span></span>
<span data-ttu-id="91224-104">Ebben a témakörben elsajátíthatja lesz egy Azure Resource Manager-sablon, amely telepíti az Azure Web Apps a Redis gyorsítótár létrehozása.</span><span class="sxs-lookup"><span data-stu-id="91224-104">In this topic, you will learn how to create an Azure Resource Manager template that deploys an Azure Web App with Redis cache.</span></span> <span data-ttu-id="91224-105">Megtudhatja, hogyan határozza meg, mely erőforrásokat központilag telepíti, és hogyan adhat meg a paramétereket, amelyek megadott, amikor a központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="91224-105">You will learn how to define which resources are deployed and how to define parameters that are specified when the deployment is executed.</span></span> <span data-ttu-id="91224-106">Ez a sablont használhatja a saját környezeteiben, vagy testre is szabhatja a saját követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="91224-106">You can use this template for your own deployments, or customize it to meet your requirements.</span></span>

<span data-ttu-id="91224-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="91224-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="91224-108">Tekintse meg a teljes sablon [webes alkalmazás a Redis Cache sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="91224-108">For the complete template, see [Web App with Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span></span>

## <a name="what-you-will-deploy"></a><span data-ttu-id="91224-109">Mit fog üzembe helyezni</span><span class="sxs-lookup"><span data-stu-id="91224-109">What you will deploy</span></span>
<span data-ttu-id="91224-110">Ha a sablonban telepíti:</span><span class="sxs-lookup"><span data-stu-id="91224-110">In this template, you will deploy:</span></span>

* <span data-ttu-id="91224-111">Azure Web App</span><span class="sxs-lookup"><span data-stu-id="91224-111">Azure Web App</span></span>
* <span data-ttu-id="91224-112">Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="91224-112">Azure Redis Cache.</span></span>

<span data-ttu-id="91224-113">Az automatikus üzembe helyezéshez kattintson az alábbi gombra:</span><span class="sxs-lookup"><span data-stu-id="91224-113">To run the deployment automatically, click the following button:</span></span>

<span data-ttu-id="91224-114">[![Üzembe helyezés az Azure-ban](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="91224-114">[![Deploy to Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters-to-specify"></a><span data-ttu-id="91224-115">Paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="91224-115">Parameters to specify</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a><span data-ttu-id="91224-116">Nevek változói</span><span class="sxs-lookup"><span data-stu-id="91224-116">Variables for names</span></span>
<span data-ttu-id="91224-117">Ez a sablon nevét az erőforrások létrehozásához változókat használ.</span><span class="sxs-lookup"><span data-stu-id="91224-117">This template uses variables to construct names for the resources.</span></span> <span data-ttu-id="91224-118">Használja a [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) függvény értéket összeállítani az erőforráscsoport azonosítója alapján.</span><span class="sxs-lookup"><span data-stu-id="91224-118">It uses the [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) function to construct a value based on the resource group id.</span></span>

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a><span data-ttu-id="91224-119">Üzembe helyezendő erőforrások</span><span class="sxs-lookup"><span data-stu-id="91224-119">Resources to deploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a><span data-ttu-id="91224-120">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="91224-120">Redis Cache</span></span>
<span data-ttu-id="91224-121">A webalkalmazásban használt Azure Redis Cache hoz létre.</span><span class="sxs-lookup"><span data-stu-id="91224-121">Creates the Azure Redis Cache that is used with the web app.</span></span> <span data-ttu-id="91224-122">A gyorsítótár neve van megadva az a **cacheName** változó.</span><span class="sxs-lookup"><span data-stu-id="91224-122">The name of the cache is specified in the **cacheName** variable.</span></span>

<span data-ttu-id="91224-123">A sablon létrehozza a gyorsítótár az erőforráscsoporttal megegyező ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="91224-123">The template creates the cache in the same location as the resource group.</span></span>

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a><span data-ttu-id="91224-124">Webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="91224-124">Web app</span></span>
<span data-ttu-id="91224-125">A megadott nevű létrehozza a webalkalmazást a **webSiteName** változó.</span><span class="sxs-lookup"><span data-stu-id="91224-125">Creates the web app with name specified in the **webSiteName** variable.</span></span>

<span data-ttu-id="91224-126">Figyelje meg, hogy a webes alkalmazás van konfigurálva, amelyek lehetővé teszik, hogy a Redis Cache használata app beállításának tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="91224-126">Notice that the web app is configured with app setting properties that enable it to work with the Redis Cache.</span></span> <span data-ttu-id="91224-127">A beállítások dinamikusan jönnek létre alkalmazást a telepítés során megadott értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="91224-127">This app settings are dynamically created based on values provided during deployment.</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a><span data-ttu-id="91224-128">Az üzembe helyezést futtató parancsok</span><span class="sxs-lookup"><span data-stu-id="91224-128">Commands to run deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="91224-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="91224-129">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="91224-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="91224-130">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
