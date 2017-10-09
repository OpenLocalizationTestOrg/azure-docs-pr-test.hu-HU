---
title: az Azure PowerShell aaaExport Resource Manager-sablon |} Microsoft Docs
description: "Használja az Azure Resource Manager és az Azure PowerShell tooexport egy sablon, egy erőforráscsoportot."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 9a239b7bce8209326c0e267a4d3d69f7014bdaed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a>A PowerShell-lel Azure Resource Manager-sablonok exportálása

Erőforrás-kezelő lehetővé teszi a Resource Manager-sablon a meglévő erőforrást az előfizetésében tooexport. A létrehozott sablon toolearn kapcsolatos hello sablon szintaxis vagy tooautomate hello a megoldás újbóli telepítését igény szerint is használhatja.

Fontos, hogy nincsenek két különböző módon tooexport sablon toonote:

* Hello tényleges sablon, amelyet a központi telepítéshez használt exportálhatja. hello exportált sablon tartalmaz minden hello paraméterek és változók pontosan úgy, mint ahogy az eredeti sablon hello. Ez a módszer akkor hasznos, ha egy sablon tooretrieve van szüksége.
* Exportálhatja a sablont, amely hello hello erőforráscsoport aktuális állapotát jeleníti meg. hello exportált sablon nem alapul, amely a központi telepítéshez használt sablonokat. Ehelyett az alkalmazás létrehozza a sablont, amely hello erőforráscsoportban található egy pillanatfelvétel. hello exportált sablonjának sok változtatható értékek és valószínűleg nem annyi megadott paraméterek általában határozzák meg. Ez a módszer akkor hasznos, ha hello erőforráscsoport módosította. A továbbiakban létre kell toocapture hello erőforráscsoport sablonként.

Ebben a témakörben mind a két megoldást bemutatjuk.

## <a name="deploy-a-solution"></a>A megoldás üzembe helyezéséhez

mindkét tooillustrate megközelíti a sablon exportálása, először megoldás tooyour előfizetés telepítése. Ha már van erőforráscsoport, amelyet az tooexport előfizetését, nincs toodeploy ebben a megoldásban. Azonban hello a cikk hátralévő része toohello sablon ehhez a megoldáshoz hivatkozik. hello a példaként megadott parancsfájlt a storage-fiók telepíti.

```powershell
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a>Menti a sablont a központi telepítés előzményei

Beolvasható egy sablont az üzembe helyezési előzményeket hello segítségével [mentés-AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) parancsot. a következő példa menti hello sablont, amely korábban központilag hello:

```powershell
Save-AzureRmResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

Hello sablon hello helyét adja vissza.

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

Nyissa meg hello fájlt, és figyelje meg, hogy a rendszer hello pontos sablon központi telepítéshez használt. hello paraméterek és változók megfelelő hello sablont a Githubból. Ez a sablon központilag telepítheti.

## <a name="export-resource-group-as-template"></a>Erőforráscsoportok exportálása sablonként

Egy sablon fogadása hello üzembe helyezési előzményeket, helyett a sablont, amely hello az erőforráscsoport aktuális állapotát jeleníti meg hello segítségével kérheti le [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) parancsot. Ez a parancs használni, amikor sok módosítások tooyour erőforráscsoport el, és nincs meglévő sablon változtatásainak hello jelöli.

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleGroup
```

Hello sablon hello helyét adja vissza.

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

Nyissa meg hello fájlt, és figyelje meg, hogy nem egyezik a Githubon hello sablont. Különböző paraméterek és változók nem rendelkezik. hello tárolási SKU és hely érték is kódolt toovalues. hello alábbi példa bemutatja hello exportált sablon, de a sablon némileg eltérő névvel rendelkezik:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccounts_nf3mvst4nqb36standardsa_name": {
      "defaultValue": null,
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[parameters('storageAccounts_nf3mvst4nqb36standardsa_name')]",
      "apiVersion": "2016-01-01",
      "location": "southcentralus",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Ez a sablon központilag telepítheti, de egy egyedi nevet a tárfiók hello találgatás igényel. a paraméter neve hello kis mértékben eltér.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a>Exportált sablon személyre szabása

Módosíthatja a sablon toomake azt toouse egyszerűbb és rugalmasabb. további helyeket, a módosítás hello hely tulajdonság toouse tooallow hello hello erőforráscsoport és ugyanazon a helyen:

```json
"location": "[resourceGroup().location]",
```

tooavoid rendelkező tooguess tárfiókot, távolítsa el a hello paraméter hello a tárfiók nevének uniques nevét. Tárolási névutótag, és egy tárolási SKU paraméter hozzáadása:

```json
"parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
},
```

Adja hozzá egy változó, amely hello tárfióknév hello uniqueString függvénnyel hoz létre:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Hello hello tárolási fiók toohello változó nevének megadása:

```json
"name": "[variables('storageAccountName')]",
```

Hello SKU toohello paraméter beállítása:

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

A sablon most a következőképpen néz ki:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), parameters('storageSuffix'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "[parameters('storageAccountType')]",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Hello módosított sablon újratelepítése.

## <a name="next-steps"></a>Következő lépések
* Hello portál tooexport egy sablon használatával kapcsolatos információkért lásd: [Azure Resource Manager-sablonok exportálása létező erőforrásokból](resource-manager-export-template.md).
* a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).
* Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).
