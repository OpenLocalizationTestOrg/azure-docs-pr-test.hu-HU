---
title: "a Resource Manager-sablonok aaaCreate művelet csoportok |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate művelet szerint kell csoportosítani a Azure Resource Manager-sablonnal."
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
ms.openlocfilehash: 9902b33cad99bd99b3deda0cf6f4ff12278c89c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a><span data-ttu-id="9285c-103">A művelet-csoport létrehozása a Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="9285c-103">Create an action group with a Resource Manager template</span></span>
<span data-ttu-id="9285c-104">Ez a cikk bemutatja, hogyan toouse egy [Azure Resource Manager sablon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure művelet csoportok.</span><span class="sxs-lookup"><span data-stu-id="9285c-104">This article shows you how toouse an [Azure Resource Manager template](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure action groups.</span></span> <span data-ttu-id="9285c-105">Sablonok segítségével automatikusan állíthatja be, amelyek felhasználhatók a meghatározott típusú riasztások művelet csoportok.</span><span class="sxs-lookup"><span data-stu-id="9285c-105">By using templates, you can automatically set up action groups that can be reused in certain types of alerts.</span></span> <span data-ttu-id="9285c-106">E művelet csoportok győződjön meg arról, hogy az összes hello megfelelő felek értesítést kap figyelmeztetést.</span><span class="sxs-lookup"><span data-stu-id="9285c-106">These action groups ensure that all hello correct parties are notified when an alert is triggered.</span></span>

<span data-ttu-id="9285c-107">hello alapvető lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="9285c-107">hello basic steps are:</span></span>

1. <span data-ttu-id="9285c-108">Hozzon létre egy sablont, amely leírja, hogyan toocreate hello művelet csoport JSON-fájlként.</span><span class="sxs-lookup"><span data-stu-id="9285c-108">Create a template as a JSON file that describes how toocreate hello action group.</span></span>

2. <span data-ttu-id="9285c-109">Hello sablon üzembe helyezése a [bármely olyan telepítési módszerrel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="9285c-109">Deploy hello template by using [any deployment method](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).</span></span>

<span data-ttu-id="9285c-110">Azt írja le, hogyan toocreate a Resource Manager-sablon esetében a művelet csoportosítani, amelyben hello művelet definíciók is változtatható hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="9285c-110">First, we describe how toocreate a Resource Manager template for an action group where hello action definitions are hard-coded in hello template.</span></span> <span data-ttu-id="9285c-111">Azt ismerteti, hogyan toocreate egy sablont, amely hello webhook konfigurációs adatok bemeneti paraméterek hello sablon telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="9285c-111">Second, we describe how toocreate a template that takes hello webhook configuration information as input parameters when hello template is deployed.</span></span>

## <a name="resource-manager-templates-for-an-action-group"></a><span data-ttu-id="9285c-112">Egy művelet csoport Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="9285c-112">Resource Manager templates for an action group</span></span>

<span data-ttu-id="9285c-113">egy művelet csoport Resource Manager sablonnal toocreate, hello típusú erőforrás létrehozása `Microsoft.Insights/actionGroups`.</span><span class="sxs-lookup"><span data-stu-id="9285c-113">toocreate an action group by using a Resource Manager template, you create a resource of hello type `Microsoft.Insights/actionGroups`.</span></span> <span data-ttu-id="9285c-114">Majd adja meg az összes kapcsolódó tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="9285c-114">Then you fill in all related properties.</span></span> <span data-ttu-id="9285c-115">Az alábbiakban a két minta sablonok művelet-csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9285c-115">Here are two sample templates that create an action group.</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within hello Resource Group) for hello Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for hello Action group."
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
        "description": "Unique name (within hello Resource Group) for hello Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for hello Action group."
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


## <a name="next-steps"></a><span data-ttu-id="9285c-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9285c-116">Next steps</span></span>
* <span data-ttu-id="9285c-117">További információ [művelet csoportok](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="9285c-117">Learn more about [action groups](monitoring-action-groups.md).</span></span>
* <span data-ttu-id="9285c-118">További információ [riasztások](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="9285c-118">Learn more about [alerts](monitoring-overview-alerts.md).</span></span>
* <span data-ttu-id="9285c-119">Megtudhatja, hogyan tooadd [Resource Manager sablonnal riasztások](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="9285c-119">Learn how tooadd [alerts by using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
