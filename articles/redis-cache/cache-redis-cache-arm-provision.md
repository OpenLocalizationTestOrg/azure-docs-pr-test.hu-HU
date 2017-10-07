---
title: "egy Redis gyorsítótárhoz Azure Resource Manager használatával aaaProvision |} Microsoft Docs"
description: "Azure Resource Manager sablon toodeploy Azure Redis Cache használata."
services: app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: 46e7b3b2493ac51dbe6bab0b086304802afc5d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-redis-cache-using-a-template"></a><span data-ttu-id="b215c-103">Redis Cache létrehozása sablon használatával</span><span class="sxs-lookup"><span data-stu-id="b215c-103">Create a Redis Cache using a template</span></span>
<span data-ttu-id="b215c-104">Ebben a témakörben elsajátíthatja, hogyan toocreate Azure Resource Manager-sablon, amely telepíti az Azure Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="b215c-104">In this topic, you learn how toocreate an Azure Resource Manager template that deploys an Azure Redis Cache.</span></span> <span data-ttu-id="b215c-105">egy meglévő tárolási fiók tookeep diagnosztikai adatok hello gyorsítótár is használható.</span><span class="sxs-lookup"><span data-stu-id="b215c-105">hello cache can be used with an existing storage account tookeep diagnostic data.</span></span> <span data-ttu-id="b215c-106">Azt is megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="b215c-106">You also learn how toodefine which resources are deployed and how toodefine parameters that are specified when hello deployment is executed.</span></span> <span data-ttu-id="b215c-107">Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="b215c-107">You can use this template for your own deployments, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="b215c-108">Diagnosztikai beállítások megosztott jelenleg az összes gyorsítótárak hello ugyanabban a régióban az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b215c-108">Currently, diagnostic settings are shared for all caches in hello same region for a subscription.</span></span> <span data-ttu-id="b215c-109">Minden más gyorsítótárak hello régióban hello régióban egy gyorsítótár frissítése hatással van.</span><span class="sxs-lookup"><span data-stu-id="b215c-109">Updating one cache in hello region affects all other caches in hello region.</span></span>

<span data-ttu-id="b215c-110">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b215c-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="b215c-111">Hello teljes sablon, lásd: [Redis Cache sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="b215c-111">For hello complete template, see [Redis Cache template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).</span></span>

> [!NOTE]
> <span data-ttu-id="b215c-112">Új hello Resource Manager-sablonok [prémium csomagban](cache-premium-tier-intro.md) érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b215c-112">Resource Manager templates for hello new [Premium tier](cache-premium-tier-intro.md) are available.</span></span> 
> 
> * [<span data-ttu-id="b215c-113">Prémium szintű Redis gyorsítótárat létrehozni fürtözési</span><span class="sxs-lookup"><span data-stu-id="b215c-113">Create a Premium Redis Cache with clustering</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [<span data-ttu-id="b215c-114">Hozzon létre Redis Cache prémium adatmegőrzés</span><span class="sxs-lookup"><span data-stu-id="b215c-114">Create Premium Redis Cache with data persistence</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [<span data-ttu-id="b215c-115">A virtuális hálózat és a választható csoportosítási Redis Cache prémium létrehozása</span><span class="sxs-lookup"><span data-stu-id="b215c-115">Create Premium Redis Cache with VNet and optional clustering</span></span>](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> <span data-ttu-id="b215c-116">toocheck hello legújabb sablonok, lásd: [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/) keresse meg a `Redis Cache`.</span><span class="sxs-lookup"><span data-stu-id="b215c-116">toocheck for hello latest templates, see [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/) and search for `Redis Cache`.</span></span>
> 
> 

## <a name="what-you-will-deploy"></a><span data-ttu-id="b215c-117">Mit fog üzembe helyezni</span><span class="sxs-lookup"><span data-stu-id="b215c-117">What you will deploy</span></span>
<span data-ttu-id="b215c-118">Ez a sablon telepíteni fogja az Azure Redis Cache, amely diagnosztikai adatok meglévő tárfiókot használ.</span><span class="sxs-lookup"><span data-stu-id="b215c-118">In this template, you will deploy an Azure Redis Cache that uses an existing storage account for diagnostic data.</span></span>

<span data-ttu-id="b215c-119">toorun telepítési hello automatikusan, kattintson a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="b215c-119">toorun hello deployment automatically, click hello following button:</span></span>

<span data-ttu-id="b215c-120">[![TooAzure telepítése](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="b215c-120">[![Deploy tooAzure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="b215c-121">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="b215c-121">Parameters</span></span>
<span data-ttu-id="b215c-122">Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="b215c-122">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="b215c-123">hello sablon tartalmazza az összes hello paraméterértékek nevű paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b215c-123">hello template includes a section called Parameters that contains all of hello parameter values.</span></span>
<span data-ttu-id="b215c-124">Ezeket az értékeket, amelyek módosítják a hello projekt telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni.</span><span class="sxs-lookup"><span data-stu-id="b215c-124">You should define a parameter for those values that vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="b215c-125">Paraméterei nem adják meg, az értékek, amelyek mindig azonos hello.</span><span class="sxs-lookup"><span data-stu-id="b215c-125">Do not define parameters for values that always stay hello same.</span></span> <span data-ttu-id="b215c-126">Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos.</span><span class="sxs-lookup"><span data-stu-id="b215c-126">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span> 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a><span data-ttu-id="b215c-127">redisCacheLocation</span><span class="sxs-lookup"><span data-stu-id="b215c-127">redisCacheLocation</span></span>
<span data-ttu-id="b215c-128">hello hello Redis gyorsítótár helyét.</span><span class="sxs-lookup"><span data-stu-id="b215c-128">hello location of hello Redis Cache.</span></span> <span data-ttu-id="b215c-129">A legjobb teljesítmény érdekében használjon hello azonos helyen, az alkalmazás toobe hello gyorsítótár használt hello.</span><span class="sxs-lookup"><span data-stu-id="b215c-129">For best performance, use hello same location as hello app toobe used with hello cache.</span></span>

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a><span data-ttu-id="b215c-130">existingDiagnosticsStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="b215c-130">existingDiagnosticsStorageAccountName</span></span>
<span data-ttu-id="b215c-131">hello neve hello meglévő tárolási fiók toouse diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="b215c-131">hello name of hello existing storage account toouse for diagnostics.</span></span> 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a><span data-ttu-id="b215c-132">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="b215c-132">enableNonSslPort</span></span>
<span data-ttu-id="b215c-133">Logikai érték, amely jelzi, hogy tooallow keresztül nem SSL portok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b215c-133">A boolean value that indicates whether tooallow access via non-SSL ports.</span></span>

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a><span data-ttu-id="b215c-134">diagnosticsStatus</span><span class="sxs-lookup"><span data-stu-id="b215c-134">diagnosticsStatus</span></span>
<span data-ttu-id="b215c-135">Egy érték, amely azt jelzi, hogy engedélyezve van-e a diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="b215c-135">A value that indicates whether diagnostics is enabled.</span></span> <span data-ttu-id="b215c-136">Használjon ON vagy OFF.</span><span class="sxs-lookup"><span data-stu-id="b215c-136">Use ON or OFF.</span></span>

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-toodeploy"></a><span data-ttu-id="b215c-137">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="b215c-137">Resources toodeploy</span></span>
### <a name="redis-cache"></a><span data-ttu-id="b215c-138">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="b215c-138">Redis Cache</span></span>
<span data-ttu-id="b215c-139">Hello Azure Redis Cache-hoz.</span><span class="sxs-lookup"><span data-stu-id="b215c-139">Creates hello Azure Redis Cache.</span></span>

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-toorun-deployment"></a><span data-ttu-id="b215c-140">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="b215c-140">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="b215c-141">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b215c-141">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a><span data-ttu-id="b215c-142">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b215c-142">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


