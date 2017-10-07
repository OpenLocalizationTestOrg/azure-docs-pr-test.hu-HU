---
title: "a Resource Manager sablonnal napló tevékenység riasztást aaaCreate |} Microsoft Docs"
description: "Értesítés az Azure-erőforrások létrehozásakor."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: ancav
ms.openlocfilehash: 0fb8aa037b9dce54ce35498622770955f2341bc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a>Tevékenység napló riasztás létrehozása egy Resource Manager sablonnal
Ez a cikk bemutatja, hogyan toouse egy [Azure Resource Manager sablon](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure tevékenység napló riasztásokat. Sablonok segítségével egyszerűen állíthat be sok riasztást, amely aktiválja az automatikus telepítési folyamat részeként az adott tevékenységre napló esemény feltételek alapján.

hello alapvető lépések a következők:

1. Hozzon létre egy sablont, amely leírja, hogyan toocreate hello tevékenység jelentkezik riasztás JSON-fájlként.

2. Hello sablon üzembe helyezése a [bármely olyan telepítési módszerrel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

## <a name="resource-manager-template-for-an-activity-log-alert"></a>Tevékenység napló riasztás Resource Manager-sablon
hello típusú erőforrás létrehozása toocreate egy napló figyelmeztetés a Resource Manager sablonnal, `microsoft.insights/activityLogAlerts`. Majd adja meg az összes kapcsolódó tulajdonságok. Ez a sablont, amely tevékenység napló riasztást hoz létre.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within hello Resource Group) for hello Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not hello alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for hello Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ] 
        },
        "actions": {
          "actionGroups": 
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

Látogasson el a [Azure gyorsindítási galéria](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) néhány példa a naplófájl értesítési sablonok számára.

## <a name="next-steps"></a>Következő lépések
- További információ [riasztások](monitoring-overview-alerts.md).
- Megtudhatja, hogyan tooadd [művelet csoportok Resource Manager sablonnal](monitoring-create-action-group-with-resource-manager-template.md).
- Ismerje meg, hogyan túl[hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés minden automatikus skálázási motor műveletek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Ismerje meg, hogyan túl[hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés összes sikertelen automatikus skálázás méretezési-a/kibővített művelete](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
