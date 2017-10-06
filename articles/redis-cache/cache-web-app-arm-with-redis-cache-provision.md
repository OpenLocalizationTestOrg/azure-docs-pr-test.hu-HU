---
title: "Webes alkalmazás a Redis Cache aaaProvision"
description: "Azure Resource Manager sablon toodeploy webes alkalmazás használata a Redis Cache."
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
ms.openlocfilehash: b95b5e230dc40c1157940c2017cba836975b6930
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a><span data-ttu-id="e9303-103">Hozzon létre egy webalkalmazást és a Redis Cache-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="e9303-103">Create a Web App plus Redis Cache using a template</span></span>
<span data-ttu-id="e9303-104">Ez a témakör megtudhatja, hogyan toocreate egy Azure Resource Manager-sablon, amely az Azure Web Apps a Redis gyorsítótár telepít.</span><span class="sxs-lookup"><span data-stu-id="e9303-104">In this topic, you will learn how toocreate an Azure Resource Manager template that deploys an Azure Web App with Redis cache.</span></span> <span data-ttu-id="e9303-105">Megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="e9303-105">You will learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="e9303-106">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="e9303-106">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="e9303-107">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e9303-107">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="e9303-108">Hello teljes sablon, lásd: [webes alkalmazás a Redis Cache sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="e9303-108">For hello complete template, see [Web App with Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).</span></span>

## <a name="what-you-will-deploy"></a><span data-ttu-id="e9303-109">Mit fog üzembe helyezni</span><span class="sxs-lookup"><span data-stu-id="e9303-109">What you will deploy</span></span>
<span data-ttu-id="e9303-110">Ha a sablonban telepíti:</span><span class="sxs-lookup"><span data-stu-id="e9303-110">In this template, you will deploy:</span></span>

* <span data-ttu-id="e9303-111">Azure Web App</span><span class="sxs-lookup"><span data-stu-id="e9303-111">Azure Web App</span></span>
* <span data-ttu-id="e9303-112">Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="e9303-112">Azure Redis Cache.</span></span>

<span data-ttu-id="e9303-113">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="e9303-113">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="e9303-114">[![TooAzure telepítése](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="e9303-114">[![Deploy tooAzure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters-toospecify"></a><span data-ttu-id="e9303-115">Paraméterek toospecify</span><span class="sxs-lookup"><span data-stu-id="e9303-115">Parameters toospecify</span></span>
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a><span data-ttu-id="e9303-116">Nevek változói</span><span class="sxs-lookup"><span data-stu-id="e9303-116">Variables for names</span></span>
<span data-ttu-id="e9303-117">Ez a sablon változók tooconstruct csoportneveket hello erőforrások használja.</span><span class="sxs-lookup"><span data-stu-id="e9303-117">This template uses variables tooconstruct names for hello resources.</span></span> <span data-ttu-id="e9303-118">Hello használ [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) tooconstruct értéket az erőforráscsoport azonosítója alapján működnek.</span><span class="sxs-lookup"><span data-stu-id="e9303-118">It uses hello [uniqueString](../azure-resource-manager/resource-group-template-functions-string.md#uniquestring) function tooconstruct a value based on the resource group id.</span></span>

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-toodeploy"></a><span data-ttu-id="e9303-119">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="e9303-119">Resources toodeploy</span></span>
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a><span data-ttu-id="e9303-120">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="e9303-120">Redis Cache</span></span>
<span data-ttu-id="e9303-121">Azure Redis Cache hello webalkalmazás használt hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e9303-121">Creates hello Azure Redis Cache that is used with hello web app.</span></span> <span data-ttu-id="e9303-122">hello gyorsítótár hello neve van megadva az hello **cacheName** változó.</span><span class="sxs-lookup"><span data-stu-id="e9303-122">hello name of hello cache is specified in hello **cacheName** variable.</span></span>

<span data-ttu-id="e9303-123">hello sablont hoz létre hello gyorsítótár hello hello erőforráscsoport és ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="e9303-123">hello template creates hello cache in hello same location as hello resource group.</span></span>

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


### <a name="web-app"></a><span data-ttu-id="e9303-124">Webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="e9303-124">Web app</span></span>
<span data-ttu-id="e9303-125">Létrehozza a hello webalkalmazást hello megadott nevű **webSiteName** változó.</span><span class="sxs-lookup"><span data-stu-id="e9303-125">Creates hello web app with name specified in hello **webSiteName** variable.</span></span>

<span data-ttu-id="e9303-126">Figyelje meg, hogy hello webalkalmazás tulajdonságokat, amelyek lehetővé teszik a Redis Cache hello toowork alkalmazás van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="e9303-126">Notice that hello web app is configured with app setting properties that enable it toowork with hello Redis Cache.</span></span> <span data-ttu-id="e9303-127">A beállítások dinamikusan jönnek létre alkalmazást a telepítés során megadott értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="e9303-127">This app settings are dynamically created based on values provided during deployment.</span></span>

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

## <a name="commands-toorun-deployment"></a><span data-ttu-id="e9303-128">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="e9303-128">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="e9303-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e9303-129">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="e9303-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e9303-130">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
