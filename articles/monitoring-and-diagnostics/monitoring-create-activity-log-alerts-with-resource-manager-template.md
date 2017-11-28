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
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a><span data-ttu-id="86c13-103">Tevékenység napló riasztás létrehozása egy Resource Manager sablonnal</span><span class="sxs-lookup"><span data-stu-id="86c13-103">Create an activity log alert with a Resource Manager template</span></span>
<span data-ttu-id="86c13-104">Ez a cikk bemutatja, hogyan toouse egy [Azure Resource Manager sablon](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure tevékenység napló riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="86c13-104">This article shows you how toouse an [Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure activity log alerts.</span></span> <span data-ttu-id="86c13-105">Sablonok segítségével egyszerűen állíthat be sok riasztást, amely aktiválja az automatikus telepítési folyamat részeként az adott tevékenységre napló esemény feltételek alapján.</span><span class="sxs-lookup"><span data-stu-id="86c13-105">By using templates, you can easily set up many alerts that activate based on specific activity log event conditions as part of your automated deployment process.</span></span>

<span data-ttu-id="86c13-106">hello alapvető lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="86c13-106">hello basic steps are:</span></span>

1. <span data-ttu-id="86c13-107">Hozzon létre egy sablont, amely leírja, hogyan toocreate hello tevékenység jelentkezik riasztás JSON-fájlként.</span><span class="sxs-lookup"><span data-stu-id="86c13-107">Create a template as a JSON file that describes how toocreate hello activity log alert.</span></span>

2. <span data-ttu-id="86c13-108">Hello sablon üzembe helyezése a [bármely olyan telepítési módszerrel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="86c13-108">Deploy hello template by using [any deployment method](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span>

## <a name="resource-manager-template-for-an-activity-log-alert"></a><span data-ttu-id="86c13-109">Tevékenység napló riasztás Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="86c13-109">Resource Manager template for an activity log alert</span></span>
<span data-ttu-id="86c13-110">hello típusú erőforrás létrehozása toocreate egy napló figyelmeztetés a Resource Manager sablonnal, `microsoft.insights/activityLogAlerts`.</span><span class="sxs-lookup"><span data-stu-id="86c13-110">toocreate an activity log alert by using a Resource Manager template, you create a resource of hello type `microsoft.insights/activityLogAlerts`.</span></span> <span data-ttu-id="86c13-111">Majd adja meg az összes kapcsolódó tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="86c13-111">Then you fill in all related properties.</span></span> <span data-ttu-id="86c13-112">Ez a sablont, amely tevékenység napló riasztást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="86c13-112">Here's a template that creates an activity log alert.</span></span>

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

<span data-ttu-id="86c13-113">Látogasson el a [Azure gyorsindítási galéria](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) néhány példa a naplófájl értesítési sablonok számára.</span><span class="sxs-lookup"><span data-stu-id="86c13-113">Visit our [Azure Quickstart gallery](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) for some examples of activity log alert templates.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86c13-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="86c13-114">Next steps</span></span>
- <span data-ttu-id="86c13-115">További információ [riasztások](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="86c13-115">Learn more about [alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="86c13-116">Megtudhatja, hogyan tooadd [művelet csoportok Resource Manager sablonnal](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="86c13-116">Learn how tooadd [action groups by using a Resource Manager template](monitoring-create-action-group-with-resource-manager-template.md).</span></span>
- <span data-ttu-id="86c13-117">Ismerje meg, hogyan túl[hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés minden automatikus skálázási motor műveletek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="86c13-117">Learn how too[create an activity log alert toomonitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="86c13-118">Ismerje meg, hogyan túl[hozzon létre egy tevékenység napló riasztási toomonitor az előfizetés összes sikertelen automatikus skálázás méretezési-a/kibővített művelete](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="86c13-118">Learn how too[create an activity log alert toomonitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
