---
title: "aaaExport Resource Manager-sablon Azure parancssori felülettel |} Microsoft Docs"
description: "Használja az Azure Resource Manager és az Azure parancssori felület tooexport egy sablon, egy erőforráscsoportot."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 2d44a0a6e9717504d4c2a01254d826679b381f22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a>Az Azure CLI Azure Resource Manager-sablonok exportálása

Erőforrás-kezelő lehetővé teszi a Resource Manager-sablon a meglévő erőforrást az előfizetésében tooexport. A létrehozott sablon toolearn kapcsolatos hello sablon szintaxis vagy tooautomate hello a megoldás újbóli telepítését igény szerint is használhatja.

Fontos, hogy nincsenek két különböző módon tooexport sablon toonote:

* Hello tényleges sablon, amelyet a központi telepítéshez használt exportálhatja. hello exportált sablon tartalmaz minden hello paraméterek és változók pontosan úgy, mint ahogy az eredeti sablon hello. Ez a módszer akkor hasznos, ha egy sablon tooretrieve van szüksége.
* Exportálhatja a sablont, amely hello hello erőforráscsoport aktuális állapotát jeleníti meg. hello exportált sablon nem alapul, amely a központi telepítéshez használt sablonokat. Ehelyett az alkalmazás létrehozza a sablont, amely hello erőforráscsoportban található egy pillanatfelvétel. hello exportált sablonjának sok változtatható értékek és valószínűleg nem annyi megadott paraméterek általában határozzák meg. Ez a módszer akkor hasznos, ha hello erőforráscsoport módosította. A továbbiakban létre kell toocapture hello erőforráscsoport sablonként.

Ebben a témakörben mind a két megoldást bemutatjuk.

## <a name="deploy-a-solution"></a>A megoldás üzembe helyezéséhez

mindkét tooillustrate megközelíti a sablon exportálása, először megoldás tooyour előfizetés telepítése. Ha már van erőforráscsoport, amelyet az tooexport előfizetését, nincs toodeploy ebben a megoldásban. Azonban hello a cikk hátralévő része toohello sablon ehhez a megoldáshoz hivatkozik. hello a példaként megadott parancsfájlt a storage-fiók telepíti.

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a>Menti a sablont a központi telepítés előzményei

Beolvasható egy sablont az üzembe helyezési előzményeket hello segítségével [az telepítési exportálása](/cli/azure/group/deployment#export) parancsot. a következő példa menti hello sablont, amely korábban központilag hello:

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

Hello sablon adja vissza. Hello JSON másolja, és menteni a fájlt. Figyelje meg, hogy a rendszer hello pontos sablon központi telepítéshez használt. hello paraméterek és változók megfelelő hello sablont a Githubból. Ez a sablon központilag telepítheti.


## <a name="export-resource-group-as-template"></a>Erőforráscsoportok exportálása sablonként

Egy sablon fogadása hello üzembe helyezési előzményeket, helyett a sablont, amely hello az erőforráscsoport aktuális állapotát jeleníti meg hello segítségével kérheti le [az exportálása](/cli/azure/group#export) parancsot. Ez a parancs használni, amikor sok módosítások tooyour erőforráscsoport el, és nincs meglévő sablon változtatásainak hello jelöli.

```azurecli
az group export --name ExampleGroup
```

Hello sablon adja vissza. Hello JSON másolja, és menteni a fájlt. Figyelje meg, hogy nem egyezik a Githubon hello sablont. Különböző paraméterek és változók nem rendelkezik. hello tárolási SKU és hely érték is kódolt toovalues. hello alábbi példa bemutatja hello exportált sablon, de a sablon némileg eltérő névvel rendelkezik:

```json
{
  "parameters": {
    "storageAccounts_mcyzaljiv7qncstandardsa_name": {
      "type": "String",
      "defaultValue": null
    }
  },
  "variables": {},
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "resources": [
    {
      "tags": {},
      "dependsOn": [],
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "type": "Microsoft.Storage/storageAccounts",
      "location": "centralus",
      "name": "[parameters('storageAccounts_mcyzaljiv7qncstandardsa_name')]",
      "properties": {}
    }
  ],
  "contentVersion": "1.0.0.0"
}
```

Ez a sablon központilag telepítheti, de egy egyedi nevet a tárfiók hello találgatás igényel. a paraméter neve hello kis mértékben eltér.

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
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