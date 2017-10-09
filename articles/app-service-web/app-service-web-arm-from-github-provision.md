---
title: "a webes alkalmazás, amely aaaDeploy kapcsolódó tooa GitHub-tárházban |} Microsoft Docs"
description: "Használja az Azure Resource Manager sablon toodeploy egy webalkalmazást, a GitHub-tárházban projektet tartalmaz."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 32739607-85fe-43c8-a4dc-1feb46d93a4d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: cephalin
ms.openlocfilehash: 8b23416c4c06a60991517e6ee4cd82bebc5a9d73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-web-app-linked-tooa-github-repository"></a>A webes alkalmazás kapcsolódó tooa GitHub-tárházban telepítése
Ebben a témakörben megismerheti, hogyan toocreate egy Azure Resource Manager-sablon, amely webes alkalmazás központi telepítését végző kapcsolódó tooa projektre a GitHub-tárházban. Megtudhatja, hogyan toodefine erőforrások vannak telepítve, és hogyan toodefine paramétereket megadott, amikor hello központi telepítés végrehajtása. Ez a sablon használhat saját rendszerekhez, vagy testre szabhatja, toomeet igényeinek.

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

Hello teljes sablon, lásd: [Web App csatolt tooGitHub sablon](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a>Mit fog üzembe helyezni
Ezzel a sablonnal a projektből a Githubon hello kódot tartalmazó webalkalmazás üzembe helyezése.

toorun telepítési hello automatikusan, kattintson a következő gombra hello:

[![TooAzure telepítése](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL
hello hello projekt toodeploy tartalmazó GitHub-tárház URL-címe Ez a paraméter alapértelmezett értéket tartalmaz, de ez az érték csak a megfelelő tooshow, hogyan tooprovide hello tárház URL-CÍMÉT. Ezt az értéket is használhatja, ha a tesztelés hello sablon, de érdemes tooprovide hello URL-cím a tárház az hello sablon használatakor.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>Fiókiroda
hello a ága hello tárház toouse hello alkalmazás központi telepítésekor. hello alapértelmezett értéke fő, de megadhat bármilyen ág hello tárházban hello nevét, hogy kívánja-e toodeploy.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-toodeploy"></a>Erőforrások toodeploy
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Webalkalmazás
A Githubon csatolt toohello projekt hello webalkalmazás hoz létre. 

Hello webalkalmazás keresztül hello hello nevének megadása **siteName** paraméter és a webalkalmazás hello keresztül hello hello hely **siteLocation** paraméter. A hello **dependsOn** elem, hello sablon meghatározása hello webalkalmazás szerint üzemeltetési terv hello szolgáltatás függ. Mivel üzemeltetési terv hello függ, hello webalkalmazás nem jön létre, amíg hello üzemeltetési terv létrehozása befejeződött. Hello **dependsOn** eleme csak a felhasznált toospecify telepítési sorrenddel. Ha nem jelöli be hello webes alkalmazást hello üzemeltetési terv függ, Azure Resource Manager megkísérli toocreate mindkét erőforrás: hello azonos idő, és egy hibaüzenet jelenik Ha hello webalkalmazás üzemeltetési terv hello előtt hozza létre.

hello webes alkalmazás is van egy gyermek erőforrás, amelyet a **erőforrások** az alábbi szakasz. A gyermek-erőforrás hello projekt telepítik hello webalkalmazás verziókövetését határozza meg. A sablonban hello verziókezelő rendszer csatolt tooa adott GitHub-tárházban. hello GitHub-tárházban hello kódra van definiálva **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** kódolnia hello tárház URL-cím lehet, ha azt szeretné, hogy a sablont, amely többször telepíti toocreate egy egyetlen projekt közben igénylő hello paraméterek minimális száma.
Rögzített megadás helyett hello tárház URL-CÍMÉT, a paraméter hozzáadása hello tárház URL-címhez, és használja ezt az értéket hello **RepoUrl** tulajdonság.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-toorun-deployment"></a>Parancsok toorun központi telepítés
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a>Azure CLI 2.0

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> Hello paraméterek JSON-fájl tartalmát, lásd: [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).
>
>

