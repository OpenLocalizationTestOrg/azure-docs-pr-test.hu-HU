---
title: "aaaDeploy erőforrások tooAzure |} Microsoft Docs"
description: "Azure PowerShell vagy Azure CLI toodeploy erőforrások tooAzure használja. a Resource Manager-sablon hello erőforrások vannak definiálva."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
ms.openlocfilehash: 0cd3f8ad45af1fb85c78899b56f6807d00b859f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-tooazure"></a>Erőforrások tooAzure telepítése

Ez a témakör bemutatja, hogyan toodeploy erőforrások tooyour Azure-előfizetés. Azure PowerShell vagy az Azure CLI toodeploy hello infrastruktúra megoldást meghatározó Resource Manager-sablon is használhatja.

Egy bevezető tooconcepts erőforrás-kezelő, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).

## <a name="steps-for-deployment"></a>Az üzembe helyezés lépései

Ez a témakör azt feltételezi, hogy elérhetővé tett hello [példa tárolási sablon](#example-storage-template) ebben a témakörben. Egy másik sablonba is használhat, de hello paraméterek át másik ebben a témakörben látható értéknél.

Egy sablon létrehozása után hello általános a sablon telepítésének lépései a következők:

1. Jelentkezzen be tooyour fiók
2. Válassza ki a hello előfizetés toouse (csak akkor szükséges, ha több előfizetéssel rendelkezik, és azt szeretné, hogy ki, amely nem hello alapértelmezett előfizetés toouse)
3. Hozzon létre egy erőforráscsoportot
4. Hello sablon üzembe helyezése
5. Az üzemelő példány állapotának ellenőrzése

hello következő szakaszok bemutatják, hogyan tooperform azokat %d{steps/ [PowerShell](#powershell) vagy [Azure CLI](#azure-cli).

## <a name="powershell"></a>PowerShell

1. tooinstall Azure PowerShell, lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview).

2. tooquickly az telepítésének első lépései, használja a következő parancsmagok hello:

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  Hello `Set-AzureRmContext` parancsmag csak akkor szükséges, ha eltérő alapértelmezett előfizetése toouse előfizetés. toosee az előfizetések és az azonosítót használja:

  ```powershell
  Get-AzureRmSubscription
  ```

3. hello központi telepítés is igénybe vehet néhány percet toocomplete. Amikor befejeződik, a következőhöz hasonló üzenet jelenik meg:

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. erőforrás csoport és a tároló fiókjához rendezték toosee telepített tooyour előfizetés, használja:

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. Sablon üzembe helyezésekor a sablon paramétereit megadhatja PowerShell-paraméterekként. hello korábbi példában nem tartalmazza a sablon paramétereket, hello alapértelmezett értékek hello sablonban használt. toodeploy egy másik tárolási fiókot, és írja be a paraméterértékek hello tároló nevének előtagja és hello tárfiók SKU, akkor:

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  Most két tárfiókja van az erőforráscsoportban. 

## <a name="azure-cli"></a>Azure CLI

1. az Azure parancssori felület tooinstall lásd [Azure CLI 2.0 telepítése](/cli/azure/install-az-cli2).

2. tooquickly az telepítésének első lépései, használja a következő parancsok hello:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  Hello `az account set` parancs csak akkor szükséges, ha eltérő alapértelmezett előfizetése toouse előfizetés. toosee az előfizetések és az azonosítót használja:

  ```azurecli
  az account list
  ```

3. hello központi telepítés is igénybe vehet néhány percet toocomplete. Amikor befejeződik, a következőhöz hasonló üzenet jelenik meg:

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. erőforrás csoport és a tároló fiókjához rendezték toosee telepített tooyour előfizetés, használja:

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. Sablon üzembe helyezésekor a sablon paramétereit megadhatja PowerShell-paraméterekként. hello korábbi példában nem tartalmazza a sablon paramétereket, hello alapértelmezett értékek hello sablonban használt. toodeploy egy másik tárolási fiókot, és írja be a paraméterértékek hello tároló nevének előtagja és hello tárfiók SKU, akkor:

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  Most két tárfiókja van az erőforráscsoportban. 

## <a name="example-storage-template"></a>Példa tárolósablon

A következő példa sablon toodeploy a tárfiók-előfizetés tooyour hello használata:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "hello value toouse for starting hello storage account name."
      }
    },
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
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Következő lépések

* PowerShell toodeploy sablonok használatával kapcsolatos részletes információkért lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](/azure/azure-resource-manager/resource-group-template-deploy).
* Az Azure parancssori felület toodeploy sablonok használatával kapcsolatos részletes információkért lásd: [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](/azure/azure-resource-manager/resource-group-template-deploy-cli).



