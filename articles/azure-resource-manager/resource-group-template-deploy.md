---
title: "a PowerShell és a sablon aaaDeploy erőforrások |} Microsoft Docs"
description: "Azure Resource Manager és az Azure PowerShell toodeploy egy erőforrások tooAzure használja. a Resource Manager-sablon hello erőforrások vannak definiálva."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 41506811ba3c2ea5df6313db70978ade50f71161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure PowerShell-lel

Ez a témakör azt ismerteti, hogyan toouse Azure PowerShell, a Resource Manager sablonok toodeploy az erőforrások tooAzure. Ha nem ismeri a hello kapcsolatos alapfogalmakat üzembe helyezése és kezelése az Azure megoldások, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).

azok a helyi fájl a számítógépre telepít hello Resource Manager-sablon, vagy egy külső egy például a GitHub-tárházban található fájl. Ebben a cikkben telepít hello sablon érhető el hello [mintasablon](#sample-template) szakasz, vagy mint [tárolási fiók sablon a Githubon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>A helyi gépről sablon telepítése

Erőforrások tooAzure telepítésekor meg:

1. Jelentkezzen be tooyour Azure-fiók
2. Hozzon létre egy erőforráscsoportot, amely hello telepített erőforrások hello tárolóként szolgál. hello erőforráscsoport nevét hello tartalmazhatnak alfanumerikus karaktereket, pontokat, aláhúzásjeleket, kötőjeleket és zárójeleket tartalmazhat. Másolatot too90 karakter lehet. Nem végződhet ponttal.
3. Toohello erőforrás csoport hello sablont, amely meghatározza a hello erőforrások toocreate telepítése

A sablon tartalmazhat, amelyek lehetővé teszik toocustomize hello telepítési paramétereit. Biztosíthatja például is lefednek értékeket (például a fejlesztői, tesztelési és éles) egy adott környezetben. hello mintasablon hello tárfiók SKU paraméter határozza meg.

a következő példa hello hoz létre egy erőforráscsoportot, és egy sablon, a helyi számítógépen telepíti:

```powershell
Login-AzureRmAccount
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

hello központi telepítés is igénybe vehet néhány percet toocomplete. A Befejezés után megjelenik egy üzenet, amely tartalmazza az hello eredmény:

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a>Külső forrásból sablon telepítése

Toostore célszerű helyett Resource Manager-sablonok a helyi számítógépen, a külső helyre. A verziókövetési tárházat (például a Githubon) sablonok tárolhat. Vagy tárolhatja őket egy Azure storage-fiók megosztott eléréséhez a szervezetében.

egy külső sablon toodeploy hello használata **TemplateUri** paraméter. Hello URI hello példa toodeploy hello minta sablont a Githubból a használata.

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

hello előző példa kell rendelkeznie a nyilvánosan elérhető URI hello sablon, amely a legtöbb környezetben működik, mivel a sablon nem érzékeny adatot kell tartalmaznia. Ha toospecify bizalmas adatok (például egy rendszergazdai jelszó) van szüksége, adja át ezt az értéket egy biztonságos paraméterben. Azonban ha nem szeretné, hogy a sablon toobe nyilvánosan elérhető, megvédheti azokat a személyes tárolót tárolja őket. A sablont, amely közös hozzáférésű jogosultságkód (SAS) jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-powershell-sas-token.md).

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

a helyi paraméterfájl toopass hello használata **TemplateParameterFile** paraméter:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

toopass külső paraméterfájl használata hello **TemplateParameterUri** paraméter:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

Beágyazott paraméterek használhatók, és egy helyi paraméter fájlt hello ugyanazt a telepítési műveletek. Például adja meg az egyes értékeket hello helyi paraméterfájl és egyéb értékek beágyazott hozzáadása a telepítés során. Ha értékeket ad meg az egyik paraméter a helyi paraméterfájl hello és a beágyazott, hello beágyazott elsőbbséget.

Azonban egy külső paraméterfájl használata esetén nem adható át más értékek vagy szövegközi vagy egy helyi fájlból. Ha megadja a paraméterfájl hello **TemplateParameterUri** paraméter, minden beágyazott paraméterek figyelmen kívül lesznek hagyva. Adja meg az összes paraméter hello külső fájlban. Ha a sablont, amely nem adhat meg hello paraméterfájl kényes értéket tartalmaz, adja hozzá az adott érték tooa kulcstároló, vagy dinamikusan adjon meg az összes paraméter értékek beágyazott.

Ha a sablon azonos nevet hello paraméterek hello PowerShell-parancsot a rendelkezésre álló hello paraméterrel tartalmaz, PowerShell hello paraméter a sablon alapján megadja a hello utótag **FromTemplate**. Például nevű paraméter **ResourceGroupName** a sablon ütközéseket hello **ResourceGroupName** hello paraméterének [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)parancsmag. Felszólító tooprovide értéket **ResourceGroupNameFromTemplate**. Ez zavart ne általában a nem azonos nevet telepítési műveleteihez használt paraméterekként hello paramétereket.

## <a name="test-a-template-deployment"></a>A sablon üzemelő példány tesztelése

tootest erőforrásokat, tényleges telepítése nélkül a sablonnal és paraméterfájlokkal értékeket használja [teszt-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment). 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Ha nincsenek hibák, a hello parancs befejezi a válaszra. Ha a rendszer hibát észlel, hello parancs hibaüzenetet ad vissza. Például kísérlet toopass hello tárfiók SKU, helytelen értéket adja vissza hello hiba a következő:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'hello provided value 'badSku' for hello template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. hello parameter value is not part of hello allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Ha a sablon szintaktikai hibát tartalmaz, a hello parancs nem tudta elemezni a hello sablon jelző hibát ad vissza. hello az üzenet azt jelzi, hello számát és a feldolgozási hiba hello pozícióját.

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

toouse teljes módban használja hello `Mode` paraméter:

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
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
* a cikkben szereplő példák hello erőforrások alapértelmezett előfizetése tooa erőforráscsoport telepítése. toouse egy másik előfizetést, lásd: [több Azure-előfizetések kezeléséhez](/powershell/azure/manage-subscriptions-azureps).
* Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [Resource Manager sablon üzembe helyezési parancsfájl](resource-manager-samples-powershell-deploy.md).
* Hogyan toodefine paramétereket a sablonban: toounderstand [hello struktúra és az Azure Resource Manager-sablonok szintaxisát](resource-group-authoring-templates.md).
* Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).
* A sablont, amely a SAS-jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-powershell-sas-token.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

