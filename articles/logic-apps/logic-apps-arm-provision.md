---
title: "aaaCreate egy logikai alkalmazást az Azure-sablonnal |} Microsoft Docs"
description: "Használja az Azure Resource Manager sablon toodeploy logikai alkalmazás definiáljon munkafolyamatokat."
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; mandia
ms.openlocfilehash: efbacb534fc7f11e9b593aae4383480ce3a1752f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-logic-app-using-a-template"></a>Logikai alkalmazás létrehozása sablonból
Sablonok adjon meg egy gyorsan toouse különböző összekötők a logikai alkalmazások. A Logic apps Azure Resource Manager-sablonokat, amelyek lehetnek használt toodefine üzleti munkafolyamatok logikai alkalmazás toocreate tartalmazza. Megadhatja, hogy mely erőforrások telepítése, és hogyan toodefine paramétereket a logikai alkalmazás központi telepítésekor. Ez a sablon a saját üzleti helyzetekben használhatja, vagy testre szabhatja, toomeet igényeinek.

Hello Logic app tulajdonságok a további részletekért lásd: [Logic App munkafolyamat felügyeleti API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Hello definíció magát, tekintse meg a [Szerző logikai alkalmazás definícióiról](logic-apps-author-definitions.md). 

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

Hello teljes sablon, lásd: [Logic App-sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-deploy"></a>Mi központi telepítése
Ezen sablon esetén telepít egy logikai alkalmazást.

toorun hello telepítési automatikusan, válassza ki a következő gombra hello:  

[![TooAzure telepítése](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-toodeploy"></a>Erőforrások toodeploy
### <a name="logic-app"></a>Logikai alkalmazás
Hello logikai alkalmazást hoz létre.

hello sablonok hello logic app name a paraméter értékét használja. Beállítja a hello logic app toohello hello helyét és hello erőforráscsoport ugyanazon a helyen. 

Ez a definíció óránként egyszer fut, és a ping-üzenetek hello hello megadott helyre **testUri** paraméter. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
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
      }
    }


## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



