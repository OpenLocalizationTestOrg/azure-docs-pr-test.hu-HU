---
title: "aaaCreate első Azure Resource Manager-sablonnal |} Microsoft Docs"
description: "Egy részletes útmutató toocreating az első Azure Resource Manager-sablon. Az bemutatja, hogyan toouse hello tárolási fiók toocreate hello sablon hivatkozása."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/27/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.openlocfilehash: 92e6d6bb7094fe0e4537ee080704967862804bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a>Az első Azure Resource Manager-sablon létrehozása ás üzembe helyezése
Ez a témakör bemutatja, hogyan hello az első Azure Resource Manager-sablonok létrehozásának lépésein. Resource Manager-sablonok JSON-fájlokat, amelyek meghatározzák a megoldáshoz szükséges toodeploy hello erőforrások. toounderstand hello fogalmak társított telepítése és kezelése az Azure megoldások, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md). Ha meglévő erőforrásokat, és szeretné, hogy az adott erőforrás tooget egy sablon, lásd: [Azure Resource Manager-sablonok exportálása létező erőforrásokból](resource-manager-export-template.md).

toocreate és ellenőrzés sablonok, egy JSON-szerkesztővel kell. A [Visual Studio Code](https://code.visualstudio.com/) egy könnyen használható, nyílt forráskódú, platformfüggetlen kódszerkesztő. A Resource Manager-sablonok létrehozásához kifejezetten javasoljuk a Visual Studio Code használatát. Ez a témakör a VS Code használatát feltételezi, de használhat egyéb JSON-szerkesztőt is (pl. Visual Studio).

## <a name="prerequisites"></a>Előfeltételek

* Visual Studio Code. Ha szükséges, a következő címről telepíthető: [https://code.visualstudio.com/](https://code.visualstudio.com/).
* Azure-előfizetés. Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="create-template"></a>Sablon létrehozása

Kezdjük azokkal, amelyek a tárfiók-előfizetés tooyour telepíti egyszerű sablonokat.

1. Válassza a **File** (Fájl) > **New File** (Új fájl) lehetőséget. 

   ![Új fájl](./media/resource-manager-create-first-template/new-file.png)

2. Másolja és illessze be a fájlba a JSON-szintaxis következő hello:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   Tárfiókneveket rendelkezik, akkor tegye őket nehéz tooset több korlátozások. hello nevének kell lennie 3 és 24 karakter hosszúságú, használja csak a számokat és kisbetűket tartalmazhatnak, és egyedi. hello előző sablon által hello [uniqueString](resource-group-template-functions-string.md#uniquestring) toogenerate kivonatot működik. toogive Ez a kivonat értéke nagyobb, ami azt jelenti, hello előtag hozzáadja *tárolási*. 

3. Mentse a fájlt **azuredeploy.json** tooa helyi mappát.

   ![Sablon mentése](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a>Sablon üzembe helyezése

Akkor van kész toodeploy ezt a sablont. Használhatja a PowerShell vagy az Azure CLI toocreate egy erőforráscsoportot. Ezután telepítsen egy tárolási fiók toothat erőforráscsoportot.

* PowerShell használja a következő parancsok hello sablon tartalmazó hello mappából hello:

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* Az Azure CLI helyi telepítés esetén parancsok hello sablon tartalmazó hello mappából a következő hello használata:

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

Központi telepítés befejezése után a storage-fiók létezik-e hello erőforráscsoportban.

## <a name="deploy-template-from-cloud-shell"></a>Sablon üzembe helyezése a Cloud Shellből

Használhat [felhő rendszerhéj](../cloud-shell/overview.md) toorun hello Azure parancssori felület parancsai a sablon telepítéséhez. Azonban Ön először be kell tölteni a sablon hello fájlmegosztás be a felhő rendszerhéj. Ha még nem használta a Cloud Shellt, a telepítésével kapcsolatban lásd [Az Azure Cloud Shell áttekintése](../cloud-shell/overview.md) című cikket.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).   

2. Válassza ki a Cloud Shell-erőforráscsoportot. hello minta nem `cloud-shell-storage-<region>`.

   ![Erőforráscsoport kiválasztása](./media/resource-manager-create-first-template/select-cs-resource-group.png)

3. A felhő rendszerhéj hello storage-fiók kiválasztása

   ![Adattároló fiók kiválasztása](./media/resource-manager-create-first-template/select-storage.png)

4. Válassza a **Files** (Fájlok) lehetőséget.

   ![Fájlok kiválasztása](./media/resource-manager-create-first-template/select-files.png)

5. Válassza ki a hello fájlmegosztás felhő rendszerhéj. hello minta nem `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Fájlmegosztás kiválasztása](./media/resource-manager-create-first-template/select-file-share.png)

6. Válassza az **Add directory** (Könyvtár hozzáadása) lehetőséget.

   ![Könyvtár hozzáadása](./media/resource-manager-create-first-template/select-add-directory.png)

7. Nevezze el a könyvtárat **templates** (sablonok) néven, és válassza az **Okay** (OK) gombot.

   ![Könyvtár elnevezése](./media/resource-manager-create-first-template/name-templates.png)

8. Jelölje ki az új könyvtárat.

   ![Könyvtár kijelölése](./media/resource-manager-create-first-template/select-templates.png)

9. Válassza a **Feltöltés** lehetőséget.

   ![Feltöltés kiválasztása](./media/resource-manager-create-first-template/select-upload.png)

10. Keresse meg és töltse fel a sablont.

   ![Fájl feltöltése](./media/resource-manager-create-first-template/upload-files.png)

11. Nyissa meg hello kérdés.

   ![Cloud Shell megnyitása](./media/resource-manager-create-first-template/start-cloud-shell.png)

12. Adja meg a következő parancsok hello felhő rendszerhéj hello:

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
   ```

Központi telepítés befejezése után a storage-fiók létezik-e hello erőforráscsoportban.

## <a name="customize-hello-template"></a>Hello sablon testreszabása

hello sablon remekül működik, de nincs rugalmas. A helyileg redundáns tárolás tooSouth USA középső RÉGIÓJA mindig telepíti. hello értéke mindig *tárolási* kivonatot követ. hello sablonnal különböző helyzetek kezelésére, tooenable paraméterek toohello sablon hozzáadása.

hello következő példa bemutatja hello paraméterek szakaszban két paraméterrel. az első paraméter hello `storageSKU` lehetővé teszi a redundancia toospecify hello típusú. Hello értékek, amelyek érvényesek a tárfiók toovalues átadhatók korlátozza. Emellett egy alapértelmezett értéket is megad. a második paraméter hello `storageNamePrefix` van beállítva tooallow legfeljebb 11 karakter. A paraméter megad egy alapértelmezett értéket.

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "hello type of replication toouse for hello storage account."
    }
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

Hello a változók szakaszban nevű változó hozzáadása `storageName`. Hello előtag a hello paraméter és érték egy a hello egyesít [uniqueString](resource-group-template-functions-string.md#uniquestring) függvény. Hello használ [toLower](resource-group-template-functions-string.md#tolower) tooconvert összes karakter toolowercase működik.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

toouse ezeket az új értékeket a tárfiók módosítása hello erőforrás-definícióban:

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    "kind": "Storage",
    "location": "[resourceGroup().location]",
    "tags": {},
    "properties": {}
  }
],
```

Figyelje meg, hogy hello toohello változó hozzáadott most beállítva hello tárfiók neve. hello termékváltozat hello paraméter értékének toohello van beállítva. hello hely hello hello erőforráscsoport és ugyanazon a helyen.

Mentse a fájlt. 

Ebben a cikkben hello lépések végrehajtását követően a sablon most néz ki:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "hello type of replication toouse for hello storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a>Sablon ismételt üzembe helyezése

Telepítse újra a hello sablon, eltérő értékű.

PowerShell esetén használja az alábbi parancsot:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

Azure CLI esetén használja az alábbi parancsot:

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

A felhő rendszerhéj hello töltse fel a módosított sablon toohello fájlmegosztást. Hello meglévő fájl felülírásához. Ezután használja a következő parancs hello:

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szükség, hello erőforráscsoport törlésével telepített hello erőforrások törlése.

PowerShell esetén használja az alábbi parancsot:

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

Azure CLI esetén használja az alábbi parancsot:

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a>Következő lépések
* toolearn hello a sablonok struktúrájával kapcsolatos, kapcsolatos további információkért lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toolearn egy tárfiókot, hello tulajdonságainak kapcsolatban lásd: [tárfiókok sablonra való hivatkozást](/azure/templates/microsoft.storage/storageaccounts).
* tooview teljes sablonok számos különböző típusú megoldások, lásd: hello [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates/).
