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
# <a name="create-a-logic-app-using-a-template"></a><span data-ttu-id="7c671-103">Logikai alkalmazás létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="7c671-103">Create a Logic App using a template</span></span>
<span data-ttu-id="7c671-104">Sablonok adjon meg egy gyorsan toouse különböző összekötők a logikai alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="7c671-104">Templates provide a quick way toouse different connectors within a logic app.</span></span> <span data-ttu-id="7c671-105">A Logic apps Azure Resource Manager-sablonokat, amelyek lehetnek használt toodefine üzleti munkafolyamatok logikai alkalmazás toocreate tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7c671-105">Logic apps includes Azure Resource Manager templates for you toocreate a logic app that can be used toodefine business workflows.</span></span> <span data-ttu-id="7c671-106">Megadhatja, hogy mely erőforrások telepítése, és hogyan toodefine paramétereket a logikai alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="7c671-106">You can define which resources are deployed, and how toodefine parameters when you deploy your logic app.</span></span> <span data-ttu-id="7c671-107">Ez a sablon a saját üzleti helyzetekben használhatja, vagy testre szabhatja, toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="7c671-107">You can use this template for your own business scenarios, or customize it toomeet your requirements.</span></span>

<span data-ttu-id="7c671-108">Hello Logic app tulajdonságok a további részletekért lásd: [Logic App munkafolyamat felügyeleti API](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c671-108">For more details on hello Logic app properties, see [Logic App Workflow Management API](https://msdn.microsoft.com/library/azure/mt643788.aspx).</span></span> 

<span data-ttu-id="7c671-109">Hello definíció magát, tekintse meg a [Szerző logikai alkalmazás definícióiról](logic-apps-author-definitions.md).</span><span class="sxs-lookup"><span data-stu-id="7c671-109">For examples of hello definition itself, see [Author Logic App definitions](logic-apps-author-definitions.md).</span></span> 

<span data-ttu-id="7c671-110">Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7c671-110">For more information about creating templates, see [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="7c671-111">Hello teljes sablon, lásd: [Logic App-sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="7c671-111">For hello complete template, see [Logic App template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).</span></span>

## <a name="what-you-deploy"></a><span data-ttu-id="7c671-112">Mi központi telepítése</span><span class="sxs-lookup"><span data-stu-id="7c671-112">What you deploy</span></span>
<span data-ttu-id="7c671-113">Ezen sablon esetén telepít egy logikai alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7c671-113">With this template, you deploy a logic app.</span></span>

<span data-ttu-id="7c671-114">toorun hello telepítési automatikusan, válassza ki a következő gombra hello:</span><span class="sxs-lookup"><span data-stu-id="7c671-114">toorun hello deployment automatically, select hello following button:</span></span>  

<span data-ttu-id="7c671-115">[![TooAzure telepítése](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="7c671-115">[![Deploy tooAzure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)</span></span>

## <a name="parameters"></a><span data-ttu-id="7c671-116">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="7c671-116">Parameters</span></span>
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a><span data-ttu-id="7c671-117">testUri</span><span class="sxs-lookup"><span data-stu-id="7c671-117">testUri</span></span>
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-toodeploy"></a><span data-ttu-id="7c671-118">Erőforrások toodeploy</span><span class="sxs-lookup"><span data-stu-id="7c671-118">Resources toodeploy</span></span>
### <a name="logic-app"></a><span data-ttu-id="7c671-119">Logikai alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7c671-119">Logic app</span></span>
<span data-ttu-id="7c671-120">Hello logikai alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7c671-120">Creates hello logic app.</span></span>

<span data-ttu-id="7c671-121">hello sablonok hello logic app name a paraméter értékét használja.</span><span class="sxs-lookup"><span data-stu-id="7c671-121">hello templates uses a parameter value for hello logic app name.</span></span> <span data-ttu-id="7c671-122">Beállítja a hello logic app toohello hello helyét és hello erőforráscsoport ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="7c671-122">It sets hello location of hello logic app toohello same location as hello resource group.</span></span> 

<span data-ttu-id="7c671-123">Ez a definíció óránként egyszer fut, és a ping-üzenetek hello hello megadott helyre **testUri** paraméter.</span><span class="sxs-lookup"><span data-stu-id="7c671-123">This particular definition runs once an hour, and pings hello location specified in hello **testUri** parameter.</span></span> 

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


## <a name="commands-toorun-deployment"></a><span data-ttu-id="7c671-124">Parancsok toorun központi telepítés</span><span class="sxs-lookup"><span data-stu-id="7c671-124">Commands toorun deployment</span></span>
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a><span data-ttu-id="7c671-125">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7c671-125">PowerShell</span></span>
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a><span data-ttu-id="7c671-126">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7c671-126">Azure CLI</span></span>
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



