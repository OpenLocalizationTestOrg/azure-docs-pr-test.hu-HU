---
title: "az Azure CLI és sablon aaaDeploy erőforrások |} Microsoft Docs"
description: "Azure Resource Manager és az Azure parancssori felület toodeploy egy erőforrások tooAzure használja. a Resource Manager-sablon hello erőforrások vannak definiálva."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: tomfitz
ms.openlocfilehash: 9f8bb9a8720399390a407030d2d32bcd97d32f13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure parancssori felületével

Ez a témakör azt ismerteti, hogyan toouse Azure CLI 2.0 a Resource Manager sablonok toodeploy az erőforrások tooAzure. Ha nem ismeri a hello kapcsolatos alapfogalmakat üzembe helyezése és kezelése az Azure megoldások, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).  

azok a helyi fájl a számítógépre telepít hello Resource Manager-sablon, vagy egy külső egy például a GitHub-tárházban található fájl. Ez a cikk központi telepítését hello sablon érhető el hello [mintasablon](#sample-template) szakasz, vagy regisztrációja, mivel egy [tárolási fiók sablon a Githubon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

Ha nincs telepítve az Azure parancssori felület, használhatja a hello [felhő rendszerhéj](#deploy-template-from-cloud-shell).

## <a name="deploy-local-template"></a>Helyi sablon üzembe helyezése

Erőforrások tooAzure telepítésekor meg:

1. Jelentkezzen be tooyour Azure-fiók
2. Hozzon létre egy erőforráscsoportot, amely hello telepített erőforrások hello tárolóként szolgál. hello erőforráscsoport nevét hello tartalmazhatnak alfanumerikus karaktereket, pontokat, aláhúzásjeleket, kötőjeleket és zárójeleket tartalmazhat. Másolatot too90 karakter lehet. Nem végződhet ponttal.
3. Toohello erőforrás csoport hello sablont, amely meghatározza a hello erőforrások toocreate telepítése

A sablon tartalmazhat, amelyek lehetővé teszik toocustomize hello telepítési paramétereit. Biztosíthatja például is lefednek értékeket (például a fejlesztői, tesztelési és éles) egy adott környezetben. hello mintasablon hello tárfiók SKU paraméter határozza meg. 

a következő példa hello hoz létre egy erőforráscsoportot, és egy sablon, a helyi számítógépen telepíti:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

hello központi telepítés is igénybe vehet néhány percet toocomplete. A Befejezés után megjelenik egy üzenet, amely tartalmazza az hello eredmény:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-external-template"></a>Külső sablon üzembe helyezése

Toostore célszerű helyett Resource Manager-sablonok a helyi számítógépen, a külső helyre. A verziókövetési tárházat (például a Githubon) sablonok tárolhat. Vagy tárolhatja őket egy Azure storage-fiók megosztott eléréséhez a szervezetében.

egy külső sablon toodeploy hello használata **sablon-uri** paraméter. Hello URI hello példa toodeploy hello minta sablont a Githubból a használata.
   
```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters storageAccountType=Standard_GRS
```

hello előző példa kell rendelkeznie a nyilvánosan elérhető URI hello sablon, amely a legtöbb környezetben működik, mivel a sablon nem érzékeny adatot kell tartalmaznia. Ha toospecify bizalmas adatok (például egy rendszergazdai jelszó) van szüksége, adja át ezt az értéket egy biztonságos paraméterben. Azonban ha nem szeretné, hogy a sablon toobe nyilvánosan elérhető, megvédheti azokat a személyes tárolót tárolja őket. A sablont, amely közös hozzáférésű jogosultságkód (SAS) jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-cli-sas-token.md).

## <a name="deploy-template-from-cloud-shell"></a>Sablon üzembe helyezése a Cloud Shellből

Használhat [felhő rendszerhéj](../cloud-shell/overview.md) toorun hello Azure parancssori felület parancsai a sablon telepítéséhez. Azonban Ön először be kell tölteni a sablon hello fájlmegosztás be a felhő rendszerhéj. Ha még nem használta a Cloud Shellt, a telepítésével kapcsolatban lásd [Az Azure Cloud Shell áttekintése](../cloud-shell/overview.md) című cikket.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).   

2. Válassza ki a Cloud Shell-erőforráscsoportot. hello minta nem `cloud-shell-storage-<region>`.

   ![Erőforráscsoport kiválasztása](./media/resource-group-template-deploy-cli/select-cs-resource-group.png)

3. A felhő rendszerhéj hello storage-fiók kiválasztása

   ![Adattároló fiók kiválasztása](./media/resource-group-template-deploy-cli/select-storage.png)

4. Válassza a **Files** (Fájlok) lehetőséget.

   ![Fájlok kiválasztása](./media/resource-group-template-deploy-cli/select-files.png)

5. Válassza ki a hello fájlmegosztás felhő rendszerhéj. hello minta nem `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Fájlmegosztás kiválasztása](./media/resource-group-template-deploy-cli/select-file-share.png)

6. Válassza az **Add directory** (Könyvtár hozzáadása) lehetőséget.

   ![Könyvtár hozzáadása](./media/resource-group-template-deploy-cli/select-add-directory.png)

7. Nevezze el a könyvtárat **templates** (sablonok) néven, és válassza az **Okay** (OK) gombot.

   ![Könyvtár elnevezése](./media/resource-group-template-deploy-cli/name-templates.png)

8. Jelölje ki az új könyvtárat.

   ![Könyvtár kijelölése](./media/resource-group-template-deploy-cli/select-templates.png)

9. Válassza a **Feltöltés** lehetőséget.

   ![Feltöltés kiválasztása](./media/resource-group-template-deploy-cli/select-upload.png)

10. Keresse meg és töltse fel a sablont.

   ![Fájl feltöltése](./media/resource-group-template-deploy-cli/upload-files.png)

11. Nyissa meg hello kérdés.

   ![Cloud Shell megnyitása](./media/resource-group-template-deploy-cli/start-cloud-shell.png)

12. Adja meg a következő parancsok hello felhő rendszerhéj hello:

   ```azurecli
   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageAccountType=Standard_GRS
   ```

## <a name="parameter-files"></a>A paraméter fájlok

Ahelyett, hogy a parancsfájl beágyazott értékeiként paraméterek átadása, azt tapasztalhatja, könnyebben toouse hello paraméterértékek tartalmazó JSON-fájl. hello paraméterfájl kell hello a következő formátumban:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Figyelje meg, hogy hello paraméterek szakaszban tartalmazza-e a paraméter neve, amely megfelel a sablonban (storageAccountType) meghatározott hello paraméter. hello paraméterfájl hello paraméter értékét tartalmazza. Ezt az értéket automatikusan átadódik toohello sablon üzembe helyezése során. Hozzon létre különböző telepítési forgatókönyvek esetén több paraméter fájlt, és akkor továbbítja a hello megfelelő paraméter fájlban. 

Példa megelőző hello másolja, majd mentse a fájlt `storage.parameters.json`.

a helyi paraméterfájl toopass használja `@` toospecify storage.parameters.json egy helyi fájlt.

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>A sablon üzemelő példány tesztelése

tootest erőforrásokat, tényleges telepítése nélkül a sablonnal és paraméterfájlokkal értékeket használja [az csoport központi telepítésének ellenőrzése](/cli/azure/group/deployment#validate). 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

Ha nincsenek hibák, a hello parancs hello próbatelepítés információt ad vissza. Különösen figyelje meg, hogy hello **hiba** értéke null.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Ha a rendszer hibát észlel, hello parancs hibaüzenetet ad vissza. Például kísérlet toopass hello tárfiók SKU, helytelen értéket adja vissza hello hiba a következő:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'hello provided value 'badSKU' for hello template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. hello parameter value is not part of hello allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Ha a sablon szintaktikai hibát tartalmaz, a hello parancs nem tudta elemezni a hello sablon jelző hibát ad vissza. hello az üzenet azt jelzi, hello számát és a feldolgozási hiba hello pozícióját.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

toouse teljes módban használja hello `mode` paraméter:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters storageAccountType=Standard_GRS
```

## <a name="sample-template"></a>Minta sablon

hello következő sablon használható hello példák ebben a témakörben. Másolja ki és mentse azt egy storage.json nevű fájlba. toounderstand hogyan toocreate ezen sablon esetén lásd: [az első Azure Resource Manager-sablon létrehozása](resource-manager-create-first-template.md).  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a>Következő lépések
* a cikkben szereplő példák hello erőforrások alapértelmezett előfizetése tooa erőforráscsoport telepítése. toouse egy másik előfizetést, lásd: [több Azure-előfizetések kezeléséhez](/cli/azure/manage-azure-subscriptions-azure-cli).
* Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [Resource Manager sablon üzembe helyezési parancsfájl](resource-manager-samples-cli-deploy.md).
* Hogyan toodefine paramétereket a sablonban: toounderstand [hello struktúra és az Azure Resource Manager-sablonok szintaxisát](resource-group-authoring-templates.md).
* Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).
* A sablont, amely a SAS-jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-cli-sas-token.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).
