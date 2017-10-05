---
title: "Automatikus engedélyezés a Resource Manager-sablon használatával diagnosztikai beállítások |} Microsoft Docs"
description: "Útmutató Resource Manager sablon használata, amely lehetővé teszi, hogy adatfolyamként küldje el az Event hubs diagnosztikai naplók, vagy olyan tárfiókban tárolja őket a diagnosztikai beállításokat szeretne létrehozni."
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
ms.openlocfilehash: dde2435e976bbd14ca35cccc714ea21dcc5817b7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a><span data-ttu-id="bc83b-103">Automatikus engedélyezés a Resource Manager-sablon használatával erőforrás létrehozásakor diagnosztikai beállítások</span><span class="sxs-lookup"><span data-stu-id="bc83b-103">Automatically enable Diagnostic Settings at resource creation using a Resource Manager template</span></span>
<span data-ttu-id="bc83b-104">Ebben a cikkben megmutatjuk, hogyan használható egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-authoring-templates.md) létrehozásakor erőforrás diagnosztikai beállításainak konfigurálására.</span><span class="sxs-lookup"><span data-stu-id="bc83b-104">In this article we show how you can use an [Azure Resource Manager template](../azure-resource-manager/resource-group-authoring-templates.md) to configure Diagnostic Settings on a resource when it is created.</span></span> <span data-ttu-id="bc83b-105">Ez lehetővé teszi, hogy automatikusan elindítja a diagnosztikai naplók és a mérni kívánt Event Hubs tárfiókokban archiválás őket, vagy Naplóelemzési elküldi őket egy erőforrás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="bc83b-105">This enables you to automatically start streaming your Diagnostic Logs and metrics to Event Hubs, archiving them in a Storage Account, or sending them to Log Analytics when a resource is created.</span></span>

<span data-ttu-id="bc83b-106">A módszer a Resource Manager-sablon használatával diagnosztikai naplók engedélyezése az erőforrástípushoz függ.</span><span class="sxs-lookup"><span data-stu-id="bc83b-106">The method for enabling Diagnostic Logs using a Resource Manager template depends on the resource type.</span></span>

* <span data-ttu-id="bc83b-107">**Nem-számítási** erőforrások (például a hálózati biztonsági csoportok, a Logic Apps Automation) [cikkben leírt diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="bc83b-107">**Non-Compute** resources (for example, Network Security Groups, Logic Apps, Automation) use [Diagnostic Settings described in this article](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span>
* <span data-ttu-id="bc83b-108">**Számítási** (ÜVEGVATTA/LAD-alapú) erőforrások használják a [ÜVEGVATTA/LAD konfigurációs fájl ebben a cikkben leírt](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span><span class="sxs-lookup"><span data-stu-id="bc83b-108">**Compute** (WAD/LAD-based) resources use the [WAD/LAD configuration file described in this article](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span></span>

<span data-ttu-id="bc83b-109">Ebben a cikkben azt konfigurálását ismertetik diagnosztika módszerek használatával.</span><span class="sxs-lookup"><span data-stu-id="bc83b-109">In this article we describe how to configure diagnostics using either method.</span></span>

<span data-ttu-id="bc83b-110">Az alapvető lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="bc83b-110">The basic steps are as follows:</span></span>

1. <span data-ttu-id="bc83b-111">Hozzon létre egy sablont, amely leírja a erőforrás létrehozása és engedélyezése a diagnosztika JSON-fájlként.</span><span class="sxs-lookup"><span data-stu-id="bc83b-111">Create a template as a JSON file that describes how to create the resource and enable diagnostics.</span></span>
2. <span data-ttu-id="bc83b-112">[A sablon bármely olyan telepítési módszerrel telepítéséhez](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="bc83b-112">[Deploy the template using any deployment method](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

<span data-ttu-id="bc83b-113">Az alábbiakban egy példa a sablon JSON-fájl generálása nem számítási és számítási erőforrásokat kell felállításához.</span><span class="sxs-lookup"><span data-stu-id="bc83b-113">Below we give an example of the template JSON file you need to generate for non-Compute and Compute resources.</span></span>

## <a name="non-compute-resource-template"></a><span data-ttu-id="bc83b-114">Nem-számítási erőforrás sablon</span><span class="sxs-lookup"><span data-stu-id="bc83b-114">Non-Compute resource template</span></span>
<span data-ttu-id="bc83b-115">Nem számítási erőforrásokat akkor két műveletet kell végrehajtania:</span><span class="sxs-lookup"><span data-stu-id="bc83b-115">For non-Compute resources, you will need to do two things:</span></span>

1. <span data-ttu-id="bc83b-116">Paraméterek hozzáadása a paraméterek blob a tárfiók neve, a service bus Szabályazonosító, illetve a OMS Naplóelemzési munkaterület azonosítója (tárfiókokban, adatfolyamként való küldése a Event Hubs-naplókat, és/vagy naplók küldése Naplóelemzési archiválási diagnosztikai naplók engedélyezése).</span><span class="sxs-lookup"><span data-stu-id="bc83b-116">Add parameters to the parameters blob for the storage account name, service bus rule ID, and/or OMS Log Analytics workspace ID (enabling archival of Diagnostic Logs in a storage account, streaming of logs to Event Hubs, and/or sending logs to Log Analytics).</span></span>
   
    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. <span data-ttu-id="bc83b-117">Az erőforrás legyen diagnosztikai naplók engedélyezése erőforrások tömbben típusú erőforrás hozzáadása `[resource namespace]/providers/diagnosticSettings`.</span><span class="sxs-lookup"><span data-stu-id="bc83b-117">In the resources array of the resource for which you want to enable Diagnostic Logs, add a resource of type `[resource namespace]/providers/diagnosticSettings`.</span></span>
   
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

<span data-ttu-id="bc83b-118">A Tulajdonságok blob a diagnosztikai beállítások a következő [a jelen cikkben ismertetett formátum](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="bc83b-118">The properties blob for the Diagnostic Setting follows [the format described in this article](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span> <span data-ttu-id="bc83b-119">Hozzáadás a `metrics` tulajdonság lehetővé teszi, hogy ezek azonos kimenetek, feltéve, hogy erőforrás metrikáit is küldhet [az erőforrás támogatja az Azure-figyelő metrikák](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="bc83b-119">Adding the `metrics` property will enable you to also send resource metrics to these same outputs, provided that [the resource supports Azure Monitor metrics](monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="bc83b-120">Ez egy teljes példa, amely hoz létre egy logikai alkalmazást, és bekapcsolja a streaming az Eseményközpontokhoz és a tárolási tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="bc83b-120">Here is a full example that creates a Logic App and turns on streaming to Event Hubs and storage in a storage account.</span></span>

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "logicAppName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Logic App that will be created."
      }
    },
    "testUri": {
      "type": "string",
      "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
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

## <a name="compute-resource-template"></a><span data-ttu-id="bc83b-121">Számítási erőforrás sablon</span><span class="sxs-lookup"><span data-stu-id="bc83b-121">Compute resource template</span></span>
<span data-ttu-id="bc83b-122">Ahhoz, hogy a számítási erőforrás diagnosztikai, például egy virtuális gép vagy a Service Fabric-fürt, akkor kell:</span><span class="sxs-lookup"><span data-stu-id="bc83b-122">To enable diagnostics on a Compute resource, for example a Virtual Machine or Service Fabric cluster, you need to:</span></span>

1. <span data-ttu-id="bc83b-123">Az Azure Diagnostics-bővítmény hozzáadása a virtuális gép erőforrás-definícióban.</span><span class="sxs-lookup"><span data-stu-id="bc83b-123">Add the Azure Diagnostics extension to the VM resource definition.</span></span>
2. <span data-ttu-id="bc83b-124">Adja meg a tárolási fiók és/vagy az event hub paraméterként.</span><span class="sxs-lookup"><span data-stu-id="bc83b-124">Specify a storage account and/or event hub as a parameter.</span></span>
3. <span data-ttu-id="bc83b-125">Adja hozzá a WADCfg XML-fájl tartalmát a XMLCfg tulajdonság, a megfelelő escape-karaktersorozat összes XML-karakter.</span><span class="sxs-lookup"><span data-stu-id="bc83b-125">Add the contents of your WADCfg XML file into the XMLCfg property, escaping all XML characters properly.</span></span>

> [!WARNING]
> <span data-ttu-id="bc83b-126">Ezen utolsó lépésében megszerezni a helyes megkapni.</span><span class="sxs-lookup"><span data-stu-id="bc83b-126">This last step can be tricky to get right.</span></span> <span data-ttu-id="bc83b-127">[Ebben a cikkben találhat](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) , amely felosztja a diagnosztika konfigurációs séma escape-karakterrel megjelölve, és megfelelő formátumú változók példát.</span><span class="sxs-lookup"><span data-stu-id="bc83b-127">[See this article](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) for an example that splits the Diagnostics Configuration Schema into variables that are escaped and formatted correctly.</span></span>
> 
> 

<span data-ttu-id="bc83b-128">A teljes folyamat minták leírt [ebben a dokumentumban](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bc83b-128">The entire process, including samples, is described [in this document](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc83b-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc83b-129">Next Steps</span></span>
* [<span data-ttu-id="bc83b-130">További tudnivalók az Azure diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="bc83b-130">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="bc83b-131">Event hubs az Azure diagnosztikai naplók adatfolyam</span><span class="sxs-lookup"><span data-stu-id="bc83b-131">Stream Azure Diagnostic Logs to Event Hubs</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)

