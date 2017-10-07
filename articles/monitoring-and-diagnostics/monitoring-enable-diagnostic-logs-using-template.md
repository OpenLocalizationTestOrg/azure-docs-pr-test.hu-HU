---
title: "aaaAutomatically engedélyezze a diagnosztikát a Resource Manager-sablonnal |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse a Resource Manager sablon toocreate diagnosztikai beállítások, amely lehetővé teszi a toostream a diagnosztikai naplók tooEvent hubok, vagy olyan tárfiókban tárolja őket."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a8a88a8c-4a48-4df6-8f7e-d90634d39c57
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/14/2017
ms.author: johnkem
ms.openlocfilehash: 8f38731107029928029c6d940da7bd076fea5d49
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Automatikus engedélyezés a Resource Manager-sablon használatával erőforrás létrehozásakor diagnosztikai beállítások
Ebben a cikkben megmutatjuk, hogyan használható egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure diagnosztikai beállítások létrehozásakor erőforráson. Ez lehetővé teszi a diagnosztikai naplók és a metrikák tooEvent hubok, tárfiókokban archiválás őket, vagy elküldené tooLog Analytics erőforrás létrehozásakor streamelési tooautomatically start.

a Resource Manager-sablon használatával diagnosztikai naplók engedélyezése hello metódus hello erőforrástípus függ.

* **Nem-számítási** erőforrások (például a hálózati biztonsági csoportok, a Logic Apps Automation) [cikkben leírt diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
* **Számítási** (ÜVEGVATTA/LAD-alapú) erőforrások használják hello [ÜVEGVATTA/LAD konfigurációs fájl ebben a cikkben leírt](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

Ez a cikk azt ismerteti hogyan tooconfigure diagnosztika módszerek használatával.

hello alapvető lépések a következők:

1. Hozzon létre egy sablont, amely leírja, hogyan toocreate hello erőforrás, és engedélyezze a diagnosztika JSON-fájlként.
2. [Bármely olyan telepítési módszerrel hello sablon üzembe helyezése](../azure-resource-manager/resource-group-template-deploy.md).

Az alábbiakban felállításához hello sablon JSON-fájl nem számítási és számítási erőforrásokat kell toogenerate példát.

## <a name="non-compute-resource-template"></a>Nem-számítási erőforrás sablon
Nem számítási erőforrásokat toodo két dolgot kell:

1. Adjon hozzá paramétereket toohello paraméterek blob hello tárfiók neve, a service bus Szabályazonosító és/vagy a OMS Naplóelemzési munkaterület azonosítója (tárfiókokban, adatfolyamként való küldése a naplók tooEvent hubok, és/vagy küld naplókat tooLog Analytics archiválási diagnosztikai naplók engedélyezése).
   
    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for hello Service Bus Namespace in which hello Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for hello Log Analytics workspace toowhich logs will be sent."
      }
    }
    ```
2. Hello erőforrások tömb hello erőforrás legyen tooenable diagnosztikai naplók, típusú erőforrás hozzáadása `[resource namespace]/providers/diagnosticSettings`.
   
    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ],
          "metrics": [
            {
              "timeGrain": "PT1M",
              "enabled": true,
              "retentionPolicy": {
                "enabled": false,
                "days": 0
              }
            }
          ]
        }
      }
    ]
    ```

hello hello diagnosztikai beállításának tulajdonságai blob a következő [cikkben leírt hello formátum](https://msdn.microsoft.com/library/azure/dn931931.aspx). Hozzáadását hello `metrics` tulajdonság lehetővé teszi a tooalso küldési erőforrás metrikáit toothese azonos kimenete, feltéve, hogy [hello erőforrás támogatja az Azure-figyelő metrikák](monitoring-supported-metrics.md).

Ez egy teljes példa, amely létrehoz egy logikai alkalmazást, és bekapcsolja a streaming tooEvent Hubs és a tárolás egy tárfiókot.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of hello Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for hello Service Bus Namespace in which hello Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for hello Log Analytics workspace toowhich logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[parameters('logicAppName')]",
      "apiVersion": "2016-06-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Logic/workflows', parameters('logicAppName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "WorkflowRuntime",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ],
            "metrics": [
              {
                "timeGrain": "PT1M",
                "enabled": true,
                "retentionPolicy": {
                  "enabled": false,
                  "days": 0
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Számítási erőforrás sablon
tooenable diagnosztika számítási erőforrás, például egy virtuális gép vagy a Service Fabric-fürt, a következőket kell tennie:

1. Adja hozzá a hello Azure Diagnostics bővítmény toohello VM erőforrás-definícióban.
2. Adja meg a tárolási fiók és/vagy az event hub paraméterként.
3. Adja hozzá a WADCfg XML-fájl tartalmának hello hello XMLCfg tulajdonság, megfelelő escape-karaktersorozat összes XML-karakter.

> [!WARNING]
> Ezen utolsó lépésében megadja a legbonyolultabb tooget jobbra lehet. [Ebben a cikkben találhat](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) , hogy elágazást diagnosztika konfigurációs séma hello escape-karakterrel megjelölve, és megfelelő formátumú változók be példát.
> 
> 

hello teljes, többek között a minták eljárást [ebben a dokumentumban](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók az Azure diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)
* [Az adatfolyam Azure diagnosztikai naplók tooEvent hubok](monitoring-stream-diagnostic-logs-to-event-hubs.md)

