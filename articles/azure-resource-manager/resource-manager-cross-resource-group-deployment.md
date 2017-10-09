---
title: "Azure-erőforrások toomultiple erőforráscsoportok aaaDeploy |} Microsoft Docs"
description: "Bemutatja, hogyan tootarget több mint egy Azure-erőforrás csoport központi telepítése során."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: tomfitz
ms.openlocfilehash: 93a39a26e0ca18dfcb5c6e8de95c38a64186d6de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-resources-toomore-than-one-resource-group"></a>Azure-erőforrások toomore, mint egy erőforráscsoport telepítése

Általában minden hello erőforrások telepít a sablon tooa egyetlen erőforráscsoportként működnek. Vannak azonban forgatókönyvek, ahol szeretné, hogy toodeploy erőforráscsoport együtt, de helyezze el őket a különböző erőforráscsoportokra. Például érdemes lehet toodeploy hello tartalék virtuális gép az Azure Site Recovery tooa külön erőforráscsoportot és helyet. Erőforrás-kezelő lehetővé teszi beágyazott toouse sablonok tootarget eltérő erőforráscsoportokban hello hello szülő sablon használt erőforráscsoport-nál.

hello erőforráscsoport hello életciklus tároló hello alkalmazás és az erőforrások gyűjteménye. Hello erőforráscsoport kívül hello sablon létrehozásához, és adja meg a hello erőforrás csoport tootarget üzembe helyezése során. Egy bevezető tooresource csoportok, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).

## <a name="example-template"></a>Példa sablon

tootarget egy másik erőforráscsoportban, kell használnia egy beágyazott vagy csatolt sablon üzembe helyezése során. Hello `Microsoft.Resources/deployments` erőforrástípust biztosít egy `resourceGroup` paraméter, amely lehetővé teszi, hogy egy másik erőforráscsoportban található a hello toospecify beágyazott központi telepítés. Az összes hello erőforráscsoport léteznie kell a hello központi telepítés futtatása előtt. hello következő példa telepíti két storage-fiókok – egy a telepítés során megadott hello erőforráscsoportot és egy-egy nevű erőforráscsoport `crossResourceGroupDeployment`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

Ha `resourceGroup` toohello nevét, amely nem található erőforráscsoport, hello központi telepítése sikertelen. Ha nem ad meg értéket `resourceGroup`, erőforrás-kezelő hello szülő erőforráscsoport használja.  

## <a name="deploy-hello-template"></a>Hello sablon üzembe helyezése

toodeploy hello példa sablon használható hello portál, az Azure PowerShell vagy az Azure parancssori felület. Az Azure PowerShell vagy Azure CLI-t előfordulhat, hogy 2017, vagy később kiadási kell használnia. hello példák azt feltételezik, hogy mentett hello sablon helyileg nevű fájl **crossrgdeployment.json**.

A PowerShell környezethez:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

Az Azure parancssori felület:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

Központi telepítés befejezése után két erőforráscsoport láthatja. Mindegyik erőforráscsoport egy tárfiókot.

## <a name="use-resourcegroup-function"></a>ResourceGroup() funkcióval

A tartományok közötti erőforrás-csoport központi telepítések, hello [resouceGroup() függvény](resource-group-template-functions-resource.md#resourcegroup) oldja fel a rendszer menübeállításoktól függően hogyan hello beágyazott sablon határozza meg. 

Ha egy sablon belül egy másik sablon beágyazásához resouceGroup() hello beágyazott sablonban toohello szülő erőforráscsoport szünteti meg. Egy beágyazott sablon hello a következő formátumot használja:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers tooparent resource group
    }
}
```

Ha külön sablon tooa, resouceGroup() hello csatolt sablonban toohello beágyazott erőforráscsoport oldja fel. Csatolt sablon hello a következő formátumot használja:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers toolinked resource group
    }
}
```

## <a name="next-steps"></a>Következő lépések

* Hogyan toodefine paramétereket a sablonban: toounderstand [hello struktúra és az Azure Resource Manager-sablonok szintaxisát](resource-group-authoring-templates.md).
* Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).
* A sablont, amely a SAS-jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-powershell-sas-token.md).
