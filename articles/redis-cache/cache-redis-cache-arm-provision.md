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
# <a name="create-a-redis-cache-using-a-template"></a>Redis Cache létrehozása sablon használatával
Ebben a témakörben elsajátíthatja, hogyan toocreate Azure Resource Manager-sablon, amely telepíti az Azure Redis Cache. egy meglévő tárolási fiók tookeep diagnosztikai adatok hello gyorsítótár is használható. Azt is megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása. Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.

Diagnosztikai beállítások megosztott jelenleg az összes gyorsítótárak hello ugyanabban a régióban az előfizetéshez. Minden más gyorsítótárak hello régióban hello régióban egy gyorsítótár frissítése hatással van.

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

Hello teljes sablon, lásd: [Redis Cache sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Új hello Resource Manager-sablonok [prémium csomagban](cache-premium-tier-intro.md) érhetők el. 
> 
> * [Prémium szintű Redis gyorsítótárat létrehozni fürtözési](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [Hozzon létre Redis Cache prémium adatmegőrzés](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [A virtuális hálózat és a választható csoportosítási Redis Cache prémium létrehozása](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> toocheck hello legújabb sablonok, lásd: [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/) keresse meg a `Redis Cache`.
> 
> 

## <a name="what-you-will-deploy"></a>Mit fog üzembe helyezni
Ez a sablon telepíteni fogja az Azure Redis Cache, amely diagnosztikai adatok meglévő tárfiókot használ.

toorun telepítési hello automatikusan, kattintson a következő gombra hello:

[![TooAzure telepítése](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek
Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor. hello sablon tartalmazza az összes hello paraméterértékek nevű paraméterek szakaszban.
Ezeket az értékeket, amelyek módosítják a hello projekt telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni. Paraméterei nem adják meg, az értékek, amelyek mindig azonos hello. Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
hello hello Redis gyorsítótár helyét. A legjobb teljesítmény érdekében használjon hello azonos helyen, az alkalmazás toobe hello gyorsítótár használt hello.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
hello neve hello meglévő tárolási fiók toouse diagnosztika. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
Logikai érték, amely jelzi, hogy tooallow keresztül nem SSL portok eléréséhez.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Egy érték, amely azt jelzi, hogy engedélyezve van-e a diagnosztika. Használjon ON vagy OFF.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-toodeploy"></a>Erőforrások toodeploy
### <a name="redis-cache"></a>Redis Cache
Hello Azure Redis Cache-hoz.

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



## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


