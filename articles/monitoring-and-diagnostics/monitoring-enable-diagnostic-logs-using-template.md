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
# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a><span data-ttu-id="32115-103">Automatikus engedélyezés a Resource Manager-sablon használatával erőforrás létrehozásakor diagnosztikai beállítások</span><span class="sxs-lookup"><span data-stu-id="32115-103">Automatically enable Diagnostic Settings at resource creation using a Resource Manager template</span></span>
<span data-ttu-id="32115-104">Ebben a cikkben megmutatjuk, hogyan használható egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure diagnosztikai beállítások létrehozásakor erőforráson.</span><span class="sxs-lookup"><span data-stu-id="32115-104">In this article we show how you can use an [Azure Resource Manager template](../azure-resource-manager/resource-group-authoring-templates.md) tooconfigure Diagnostic Settings on a resource when it is created.</span></span> <span data-ttu-id="32115-105">Ez lehetővé teszi a diagnosztikai naplók és a metrikák tooEvent hubok, tárfiókokban archiválás őket, vagy elküldené tooLog Analytics erőforrás létrehozásakor streamelési tooautomatically start.</span><span class="sxs-lookup"><span data-stu-id="32115-105">This enables you tooautomatically start streaming your Diagnostic Logs and metrics tooEvent Hubs, archiving them in a Storage Account, or sending them tooLog Analytics when a resource is created.</span></span>

<span data-ttu-id="32115-106">a Resource Manager-sablon használatával diagnosztikai naplók engedélyezése hello metódus hello erőforrástípus függ.</span><span class="sxs-lookup"><span data-stu-id="32115-106">hello method for enabling Diagnostic Logs using a Resource Manager template depends on hello resource type.</span></span>

* <span data-ttu-id="32115-107">**Nem-számítási** erőforrások (például a hálózati biztonsági csoportok, a Logic Apps Automation) [cikkben leírt diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span><span class="sxs-lookup"><span data-stu-id="32115-107">**Non-Compute** resources (for example, Network Security Groups, Logic Apps, Automation) use [Diagnostic Settings described in this article](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).</span></span>
* <span data-ttu-id="32115-108">**Számítási** (ÜVEGVATTA/LAD-alapú) erőforrások használják hello [ÜVEGVATTA/LAD konfigurációs fájl ebben a cikkben leírt](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span><span class="sxs-lookup"><span data-stu-id="32115-108">**Compute** (WAD/LAD-based) resources use hello [WAD/LAD configuration file described in this article](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).</span></span>

<span data-ttu-id="32115-109">Ez a cikk azt ismerteti hogyan tooconfigure diagnosztika módszerek használatával.</span><span class="sxs-lookup"><span data-stu-id="32115-109">In this article we describe how tooconfigure diagnostics using either method.</span></span>

<span data-ttu-id="32115-110">hello alapvető lépések a következők:</span><span class="sxs-lookup"><span data-stu-id="32115-110">hello basic steps are as follows:</span></span>

1. <span data-ttu-id="32115-111">Hozzon létre egy sablont, amely leírja, hogyan toocreate hello erőforrás, és engedélyezze a diagnosztika JSON-fájlként.</span><span class="sxs-lookup"><span data-stu-id="32115-111">Create a template as a JSON file that describes how toocreate hello resource and enable diagnostics.</span></span>
2. <span data-ttu-id="32115-112">[Bármely olyan telepítési módszerrel hello sablon üzembe helyezése](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="32115-112">[Deploy hello template using any deployment method](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

<span data-ttu-id="32115-113">Az alábbiakban felállításához hello sablon JSON-fájl nem számítási és számítási erőforrásokat kell toogenerate példát.</span><span class="sxs-lookup"><span data-stu-id="32115-113">Below we give an example of hello template JSON file you need toogenerate for non-Compute and Compute resources.</span></span>

## <a name="non-compute-resource-template"></a><span data-ttu-id="32115-114">Nem-számítási erőforrás sablon</span><span class="sxs-lookup"><span data-stu-id="32115-114">Non-Compute resource template</span></span>
<span data-ttu-id="32115-115">Nem számítási erőforrásokat toodo két dolgot kell:</span><span class="sxs-lookup"><span data-stu-id="32115-115">For non-Compute resources, you will need toodo two things:</span></span>

1. <span data-ttu-id="32115-116">Adjon hozzá paramétereket toohello paraméterek blob hello tárfiók neve, a service bus Szabályazonosító és/vagy a OMS Naplóelemzési munkaterület azonosítója (tárfiókokban, adatfolyamként való küldése a naplók tooEvent hubok, és/vagy küld naplókat tooLog Analytics archiválási diagnosztikai naplók engedélyezése).</span><span class="sxs-lookup"><span data-stu-id="32115-116">Add parameters toohello parameters blob for hello storage account name, service bus rule ID, and/or OMS Log Analytics workspace ID (enabling archival of Diagnostic Logs in a storage account, streaming of logs tooEvent Hubs, and/or sending logs tooLog Analytics).</span></span>
   
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
2. <span data-ttu-id="32115-117">Hello erőforrások tömb hello erőforrás legyen tooenable diagnosztikai naplók, típusú erőforrás hozzáadása `[resource namespace]/providers/diagnosticSettings`.</span><span class="sxs-lookup"><span data-stu-id="32115-117">In hello resources array of hello resource for which you want tooenable Diagnostic Logs, add a resource of type `[resource namespace]/providers/diagnosticSettings`.</span></span>
   
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

<span data-ttu-id="32115-118">hello hello diagnosztikai beállításának tulajdonságai blob a következő [cikkben leírt hello formátum](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span><span class="sxs-lookup"><span data-stu-id="32115-118">hello properties blob for hello Diagnostic Setting follows [hello format described in this article](https://msdn.microsoft.com/library/azure/dn931931.aspx).</span></span> <span data-ttu-id="32115-119">Hozzáadását hello `metrics` tulajdonság lehetővé teszi a tooalso küldési erőforrás metrikáit toothese azonos kimenete, feltéve, hogy [hello erőforrás támogatja az Azure-figyelő metrikák](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="32115-119">Adding hello `metrics` property will enable you tooalso send resource metrics toothese same outputs, provided that [hello resource supports Azure Monitor metrics](monitoring-supported-metrics.md).</span></span>

<span data-ttu-id="32115-120">Ez egy teljes példa, amely létrehoz egy logikai alkalmazást, és bekapcsolja a streaming tooEvent Hubs és a tárolás egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="32115-120">Here is a full example that creates a Logic App and turns on streaming tooEvent Hubs and storage in a storage account.</span></span>

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

## <a name="compute-resource-template"></a><span data-ttu-id="32115-121">Számítási erőforrás sablon</span><span class="sxs-lookup"><span data-stu-id="32115-121">Compute resource template</span></span>
<span data-ttu-id="32115-122">tooenable diagnosztika számítási erőforrás, például egy virtuális gép vagy a Service Fabric-fürt, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="32115-122">tooenable diagnostics on a Compute resource, for example a Virtual Machine or Service Fabric cluster, you need to:</span></span>

1. <span data-ttu-id="32115-123">Adja hozzá a hello Azure Diagnostics bővítmény toohello VM erőforrás-definícióban.</span><span class="sxs-lookup"><span data-stu-id="32115-123">Add hello Azure Diagnostics extension toohello VM resource definition.</span></span>
2. <span data-ttu-id="32115-124">Adja meg a tárolási fiók és/vagy az event hub paraméterként.</span><span class="sxs-lookup"><span data-stu-id="32115-124">Specify a storage account and/or event hub as a parameter.</span></span>
3. <span data-ttu-id="32115-125">Adja hozzá a WADCfg XML-fájl tartalmának hello hello XMLCfg tulajdonság, megfelelő escape-karaktersorozat összes XML-karakter.</span><span class="sxs-lookup"><span data-stu-id="32115-125">Add hello contents of your WADCfg XML file into hello XMLCfg property, escaping all XML characters properly.</span></span>

> [!WARNING]
> <span data-ttu-id="32115-126">Ezen utolsó lépésében megadja a legbonyolultabb tooget jobbra lehet.</span><span class="sxs-lookup"><span data-stu-id="32115-126">This last step can be tricky tooget right.</span></span> <span data-ttu-id="32115-127">[Ebben a cikkben találhat](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) , hogy elágazást diagnosztika konfigurációs séma hello escape-karakterrel megjelölve, és megfelelő formátumú változók be példát.</span><span class="sxs-lookup"><span data-stu-id="32115-127">[See this article](../virtual-machines/windows/extensions-diagnostics-template.md#diagnostics-configuration-variables) for an example that splits hello Diagnostics Configuration Schema into variables that are escaped and formatted correctly.</span></span>
> 
> 

<span data-ttu-id="32115-128">hello teljes, többek között a minták eljárást [ebben a dokumentumban](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="32115-128">hello entire process, including samples, is described [in this document](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="32115-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32115-129">Next Steps</span></span>
* [<span data-ttu-id="32115-130">További tudnivalók az Azure diagnosztikai naplók</span><span class="sxs-lookup"><span data-stu-id="32115-130">Read more about Azure Diagnostic Logs</span></span>](monitoring-overview-of-diagnostic-logs.md)
* [<span data-ttu-id="32115-131">Az adatfolyam Azure diagnosztikai naplók tooEvent hubok</span><span class="sxs-lookup"><span data-stu-id="32115-131">Stream Azure Diagnostic Logs tooEvent Hubs</span></span>](monitoring-stream-diagnostic-logs-to-event-hubs.md)

