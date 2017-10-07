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
# <a name="deploy-resources-tooazure"></a><span data-ttu-id="8d938-104">Erőforrások tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="8d938-104">Deploy resources tooAzure</span></span>

<span data-ttu-id="8d938-105">Ez a témakör bemutatja, hogyan toodeploy erőforrások tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8d938-105">This topic shows how toodeploy resources tooyour Azure subscription.</span></span> <span data-ttu-id="8d938-106">Azure PowerShell vagy az Azure CLI toodeploy hello infrastruktúra megoldást meghatározó Resource Manager-sablon is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8d938-106">You can use either Azure PowerShell or Azure CLI toodeploy a Resource Manager template that defines hello infrastructure for your solution.</span></span>

<span data-ttu-id="8d938-107">Egy bevezető tooconcepts erőforrás-kezelő, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8d938-107">For an introduction tooconcepts of Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="steps-for-deployment"></a><span data-ttu-id="8d938-108">Az üzembe helyezés lépései</span><span class="sxs-lookup"><span data-stu-id="8d938-108">Steps for deployment</span></span>

<span data-ttu-id="8d938-109">Ez a témakör azt feltételezi, hogy elérhetővé tett hello [példa tárolási sablon](#example-storage-template) ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="8d938-109">This topic assumes you are deploying hello [example storage template](#example-storage-template) in this topic.</span></span> <span data-ttu-id="8d938-110">Egy másik sablonba is használhat, de hello paraméterek át másik ebben a témakörben látható értéknél.</span><span class="sxs-lookup"><span data-stu-id="8d938-110">You can use a different template, but hello parameters you pass are different than what is shown in this topic.</span></span>

<span data-ttu-id="8d938-111">Egy sablon létrehozása után hello általános a sablon telepítésének lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="8d938-111">After creating a template, hello general steps for deploying your template are:</span></span>

1. <span data-ttu-id="8d938-112">Jelentkezzen be tooyour fiók</span><span class="sxs-lookup"><span data-stu-id="8d938-112">Log in tooyour account</span></span>
2. <span data-ttu-id="8d938-113">Válassza ki a hello előfizetés toouse (csak akkor szükséges, ha több előfizetéssel rendelkezik, és azt szeretné, hogy ki, amely nem hello alapértelmezett előfizetés toouse)</span><span class="sxs-lookup"><span data-stu-id="8d938-113">Select hello subscription toouse (only necessary if you have multiple subscriptions, and you want toouse one that is not hello default subscription)</span></span>
3. <span data-ttu-id="8d938-114">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="8d938-114">Create a resource group</span></span>
4. <span data-ttu-id="8d938-115">Hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="8d938-115">Deploy hello template</span></span>
5. <span data-ttu-id="8d938-116">Az üzemelő példány állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="8d938-116">Check your deployment status</span></span>

<span data-ttu-id="8d938-117">hello következő szakaszok bemutatják, hogyan tooperform azokat %d{steps/ [PowerShell](#powershell) vagy [Azure CLI](#azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8d938-117">hello following sections show how tooperform those steps with [PowerShell](#powershell) or [Azure CLI](#azure-cli).</span></span>

## <a name="powershell"></a><span data-ttu-id="8d938-118">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8d938-118">PowerShell</span></span>

1. <span data-ttu-id="8d938-119">tooinstall Azure PowerShell, lásd: [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8d938-119">tooinstall Azure PowerShell, see [Get started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>

2. <span data-ttu-id="8d938-120">tooquickly az telepítésének első lépései, használja a következő parancsmagok hello:</span><span class="sxs-lookup"><span data-stu-id="8d938-120">tooquickly get started with deployment, use hello following cmdlets:</span></span>

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  <span data-ttu-id="8d938-121">Hello `Set-AzureRmContext` parancsmag csak akkor szükséges, ha eltérő alapértelmezett előfizetése toouse előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8d938-121">hello `Set-AzureRmContext` cmdlet is only needed if you want toouse a subscription other than your default subscription.</span></span> <span data-ttu-id="8d938-122">toosee az előfizetések és az azonosítót használja:</span><span class="sxs-lookup"><span data-stu-id="8d938-122">toosee all your subscriptions and their IDs, use:</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="8d938-123">hello központi telepítés is igénybe vehet néhány percet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="8d938-123">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="8d938-124">Amikor befejeződik, a következőhöz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8d938-124">When it finishes, you see a message similar to:</span></span>

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. <span data-ttu-id="8d938-125">erőforrás csoport és a tároló fiókjához rendezték toosee telepített tooyour előfizetés, használja:</span><span class="sxs-lookup"><span data-stu-id="8d938-125">toosee that your resource group and storage account were deployed tooyour subscription, use:</span></span>

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. <span data-ttu-id="8d938-126">Sablon üzembe helyezésekor a sablon paramétereit megadhatja PowerShell-paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="8d938-126">You can specify template parameters as PowerShell parameters when deploying a template.</span></span> <span data-ttu-id="8d938-127">hello korábbi példában nem tartalmazza a sablon paramétereket, hello alapértelmezett értékek hello sablonban használt.</span><span class="sxs-lookup"><span data-stu-id="8d938-127">hello earlier example did not include any template parameters, so hello default values in hello template were used.</span></span> <span data-ttu-id="8d938-128">toodeploy egy másik tárolási fiókot, és írja be a paraméterértékek hello tároló nevének előtagja és hello tárfiók SKU, akkor:</span><span class="sxs-lookup"><span data-stu-id="8d938-128">toodeploy another storage account, and provide parameter values for hello storage name prefix and hello storage account SKU, use:</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  <span data-ttu-id="8d938-129">Most két tárfiókja van az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="8d938-129">You now have two storage accounts in your resource group.</span></span> 

## <a name="azure-cli"></a><span data-ttu-id="8d938-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8d938-130">Azure CLI</span></span>

1. <span data-ttu-id="8d938-131">az Azure parancssori felület tooinstall lásd [Azure CLI 2.0 telepítése](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="8d938-131">tooinstall Azure CLI, see [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

2. <span data-ttu-id="8d938-132">tooquickly az telepítésének első lépései, használja a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="8d938-132">tooquickly get started with deployment, use hello following commands:</span></span>

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  <span data-ttu-id="8d938-133">Hello `az account set` parancs csak akkor szükséges, ha eltérő alapértelmezett előfizetése toouse előfizetés.</span><span class="sxs-lookup"><span data-stu-id="8d938-133">hello `az account set` command is only needed if you want toouse a subscription other than your default subscription.</span></span> <span data-ttu-id="8d938-134">toosee az előfizetések és az azonosítót használja:</span><span class="sxs-lookup"><span data-stu-id="8d938-134">toosee all your subscriptions and their IDs, use:</span></span>

  ```azurecli
  az account list
  ```

3. <span data-ttu-id="8d938-135">hello központi telepítés is igénybe vehet néhány percet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="8d938-135">hello deployment can take a few minutes toocomplete.</span></span> <span data-ttu-id="8d938-136">Amikor befejeződik, a következőhöz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8d938-136">When it finishes, you see a message similar to:</span></span>

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. <span data-ttu-id="8d938-137">erőforrás csoport és a tároló fiókjához rendezték toosee telepített tooyour előfizetés, használja:</span><span class="sxs-lookup"><span data-stu-id="8d938-137">toosee that your resource group and storage account were deployed tooyour subscription, use:</span></span>

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. <span data-ttu-id="8d938-138">Sablon üzembe helyezésekor a sablon paramétereit megadhatja PowerShell-paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="8d938-138">You can specify template parameters as PowerShell parameters when deploying a template.</span></span> <span data-ttu-id="8d938-139">hello korábbi példában nem tartalmazza a sablon paramétereket, hello alapértelmezett értékek hello sablonban használt.</span><span class="sxs-lookup"><span data-stu-id="8d938-139">hello earlier example did not include any template parameters, so hello default values in hello template were used.</span></span> <span data-ttu-id="8d938-140">toodeploy egy másik tárolási fiókot, és írja be a paraméterértékek hello tároló nevének előtagja és hello tárfiók SKU, akkor:</span><span class="sxs-lookup"><span data-stu-id="8d938-140">toodeploy another storage account, and provide parameter values for hello storage name prefix and hello storage account SKU, use:</span></span>

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  <span data-ttu-id="8d938-141">Most két tárfiókja van az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="8d938-141">You now have two storage accounts in your resource group.</span></span> 

## <a name="example-storage-template"></a><span data-ttu-id="8d938-142">Példa tárolósablon</span><span class="sxs-lookup"><span data-stu-id="8d938-142">Example storage template</span></span>

<span data-ttu-id="8d938-143">A következő példa sablon toodeploy a tárfiók-előfizetés tooyour hello használata:</span><span class="sxs-lookup"><span data-stu-id="8d938-143">Use hello following example template toodeploy a storage account tooyour subscription:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8d938-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d938-144">Next steps</span></span>

* <span data-ttu-id="8d938-145">PowerShell toodeploy sablonok használatával kapcsolatos részletes információkért lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="8d938-145">For detailed information about using PowerShell toodeploy templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy).</span></span>
* <span data-ttu-id="8d938-146">Az Azure parancssori felület toodeploy sablonok használatával kapcsolatos részletes információkért lásd: [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](/azure/azure-resource-manager/resource-group-template-deploy-cli).</span><span class="sxs-lookup"><span data-stu-id="8d938-146">For detailed information about using Azure CLI toodeploy templates, see [Deploy resources with Resource Manager templates and Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli).</span></span>



