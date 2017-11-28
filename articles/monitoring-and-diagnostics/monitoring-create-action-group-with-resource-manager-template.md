---
title: "A művelet csoportok létrehozása a Resource Manager-sablonok |} Microsoft Docs"
description: "Útmutató: egy művelet csoport létrehozása az Azure Resource Manager-sablon használatával."
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
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 76bf353cac13f1c2169380f8dd3c1e163d4f3f41
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a><span data-ttu-id="9711e-103">A művelet-csoport létrehozása a Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="9711e-103">Create an action group with a Resource Manager template</span></span>
<span data-ttu-id="9711e-104">Ez a cikk bemutatja, hogyan használható egy [Azure Resource Manager sablon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) művelet csoportok konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="9711e-104">This article shows you how to use an [Azure Resource Manager template](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) to configure action groups.</span></span> <span data-ttu-id="9711e-105">Sablonok segítségével automatikusan állíthatja be, amelyek felhasználhatók a meghatározott típusú riasztások művelet csoportok.</span><span class="sxs-lookup"><span data-stu-id="9711e-105">By using templates, you can automatically set up action groups that can be reused in certain types of alerts.</span></span> <span data-ttu-id="9711e-106">E művelet csoportok győződjön meg arról, hogy a helyes felek értesítést kap figyelmeztetést.</span><span class="sxs-lookup"><span data-stu-id="9711e-106">These action groups ensure that all the correct parties are notified when an alert is triggered.</span></span>

<span data-ttu-id="9711e-107">Az alapvető lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="9711e-107">The basic steps are:</span></span>

1. <span data-ttu-id="9711e-108">Hozzon létre egy sablont, amely ismerteti, hogyan lehet létrehozni a műveleti csoportot JSON-fájlként.</span><span class="sxs-lookup"><span data-stu-id="9711e-108">Create a template as a JSON file that describes how to create the action group.</span></span>

2. <span data-ttu-id="9711e-109">A sablon telepítéséhez használatával [bármely olyan telepítési módszerrel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="9711e-109">Deploy the template by using [any deployment method](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span>

<span data-ttu-id="9711e-110">Először azt ismerteti, hogyan egy művelet csoport, hol találhatók a művelet definíciók kódolva a sablon a Resource Manager-sablonok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9711e-110">First, we describe how to create a Resource Manager template for an action group where the action definitions are hard-coded in the template.</span></span> <span data-ttu-id="9711e-111">Második azt ismertetik, amelyek bemeneti paraméterként webhook konfigurációs adatokat fogad a sablon telepítésekor sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9711e-111">Second, we describe how to create a template that takes the webhook configuration information as input parameters when the template is deployed.</span></span>

## <a name="resource-manager-templates-for-an-action-group"></a><span data-ttu-id="9711e-112">Egy művelet csoport Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="9711e-112">Resource Manager templates for an action group</span></span>

<span data-ttu-id="9711e-113">Művelet-csoport létrehozása a Resource Manager sablonnal, hozzon létre egy erőforrást a típusú `Microsoft.Insights/actionGroups`.</span><span class="sxs-lookup"><span data-stu-id="9711e-113">To create an action group by using a Resource Manager template, you create a resource of the type `Microsoft.Insights/actionGroups`.</span></span> <span data-ttu-id="9711e-114">Majd adja meg az összes kapcsolódó tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="9711e-114">Then you fill in all related properties.</span></span> <span data-ttu-id="9711e-115">Az alábbiakban a két minta sablonok művelet-csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9711e-115">Here are two sample templates that create an action group.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a><span data-ttu-id="9711e-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9711e-116">Next steps</span></span>
* <span data-ttu-id="9711e-117">További információ [művelet csoportok](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="9711e-117">Learn more about [action groups](monitoring-action-groups.md).</span></span>
* <span data-ttu-id="9711e-118">További információ [riasztások](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="9711e-118">Learn more about [alerts](monitoring-overview-alerts.md).</span></span>
* <span data-ttu-id="9711e-119">Ismerje meg, hogyan adhat [Resource Manager sablonnal riasztások](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="9711e-119">Learn how to add [alerts by using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
