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
# <a name="create-an-action-group-with-a-resource-manager-template"></a>A művelet-csoport létrehozása a Resource Manager-sablon
Ez a cikk bemutatja, hogyan toouse egy [Azure Resource Manager sablon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) tooconfigure művelet csoportok. Sablonok segítségével automatikusan állíthatja be, amelyek felhasználhatók a meghatározott típusú riasztások művelet csoportok. E művelet csoportok győződjön meg arról, hogy az összes hello megfelelő felek értesítést kap figyelmeztetést.

hello alapvető lépések a következők:

1. Hozzon létre egy sablont, amely leírja, hogyan toocreate hello művelet csoport JSON-fájlként.

2. Hello sablon üzembe helyezése a [bármely olyan telepítési módszerrel](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

Azt írja le, hogyan toocreate a Resource Manager-sablon esetében a művelet csoportosítani, amelyben hello művelet definíciók is változtatható hello sablonban. Azt ismerteti, hogyan toocreate egy sablont, amely hello webhook konfigurációs adatok bemeneti paraméterek hello sablon telepítésekor.

## <a name="resource-manager-templates-for-an-action-group"></a>Egy művelet csoport Resource Manager-sablonok

egy művelet csoport Resource Manager sablonnal toocreate, hello típusú erőforrás létrehozása `Microsoft.Insights/actionGroups`. Majd adja meg az összes kapcsolódó tulajdonságok. Az alábbiakban a két minta sablonok művelet-csoport létrehozása.

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


## <a name="next-steps"></a>Következő lépések
* További információ [művelet csoportok](monitoring-action-groups.md).
* További információ [riasztások](monitoring-overview-alerts.md).
* Megtudhatja, hogyan tooadd [Resource Manager sablonnal riasztások](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
