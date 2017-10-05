---
title: "A GitHub-tárházban kapcsolódó webalkalmazás üzembe helyezése |} Microsoft Docs"
description: "Az Azure Resource Manager-sablon használatával webalkalmazás üzembe helyezése a GitHub-tárházban projektet tartalmaz."
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
ms.openlocfilehash: 77064802814296d0c21f004534e4264d2f97252e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>A GitHub-tárházban kapcsolódó webalkalmazás üzembe helyezése
Ebben a témakörben, megtudhatja, hogyan egy Azure Resource Manager-sablon, amely telepít egy webalkalmazást, amely csatolva van egy GitHub-tárházban a projekt létrehozásához. Megtudhatja, hogyan határozza meg, mely erőforrásokat központilag telepíti, és hogyan adhat meg a paramétereket, amelyek megadott, amikor a központi telepítés végrehajtása. Ez a sablont használhatja a saját környezeteiben, vagy testre is szabhatja a saját követelményeinek megfelelően.

Sablonok létrehozásával kapcsolatos további információkért lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).

Tekintse meg a teljes sablon [Web App csatolt GitHub sablonhoz](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="what-you-will-deploy"></a>Mit fog üzembe helyezni
Ezzel a sablonnal a projektből a Githubon kódot tartalmazó webalkalmazás üzembe helyezése.

Az automatikus üzembe helyezéshez kattintson az alábbi gombra:

[![Üzembe helyezés az Azure-ban](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Paraméterek
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL
A projekt telepítése tartalmazó GitHub-tárházban URL-CÍMÉT. Ez a paraméter alapértelmezett értéket tartalmaz, de ez az érték csak olyan bemutatják a tárház URL-CÍMÉT adja meg. Is használhatja ezt az értéket, a sablon tesztelése során, de a rendszer szeretne biztosítani a saját tárház URL-CÍMÉT az a sablon használatakor.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>Fiókiroda
A tárház az alkalmazás telepítése során használandó helyezendő ága. Az alapértelmezett érték a főadatbázis, de megadhatja a telepíteni kívánt tárházban bármely ág nevét.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }

## <a name="resources-to-deploy"></a>Üzembe helyezendő erőforrások
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Webalkalmazás
Létrehozza a webalkalmazást, amely csatolva van a projektet a Githubról. 

A nevet a webalkalmazás keresztül megadnia a **siteName** paraméter, és a web app használatával helye a **siteLocation** paraméter. Az a **dependsOn** elem, a sablon határozza meg a webalkalmazás az üzemeltetési terv szolgáltatástól függ. Mivel a üzemeltetési terv függ, a webes alkalmazás nem jön létre, amíg a üzemeltetési terv létrehozása befejeződött. A **dependsOn** elem csak használatával adja meg a telepítési sorrendet. Ha nem jelöli be a üzemeltetési terv függ a webes alkalmazást, Azure Resource Manager megkísérli mindkét erőforrás létrehozása egy időben, és hiba jelenhet meg, ha a webalkalmazás létrehozása előtt a üzemeltetési terv.

A webes alkalmazás is van egy gyermek erőforrás, amelyet a **erőforrások** az alábbi szakasz. A gyermek-erőforrás határozza meg a projekt telepítik a webalkalmazás verziókövetését. A sablonban a verziókezelőt egy adott GitHub-tárházban van csatolva. A GitHub-tárházban van definiálva a kód **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** , előfordulhat, hogy kódolnia a tárház URL-CÍMÉT Ha azt szeretné, amely többször telepít egy sablon létrehozása a projekt közben igénylő paraméterek minimális száma.
Rögzített megadás a tárház URL-CÍMÉT, helyett paraméter hozzáadása a tárház URL-címhez és használja ezt az értéket a a **RepoUrl** tulajdonság.

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

## <a name="commands-to-run-deployment"></a>Az üzembe helyezést futtató parancsok
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json

### <a name="azure-cli-20"></a>Azure CLI 2.0

    az group deployment create -g {resource-group-name} --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json --parameters '@azuredeploy.parameters.json'

> [!NOTE] 
> A paraméterek JSON-fájl tartalmát, lásd: [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.parameters.json).
>
>

