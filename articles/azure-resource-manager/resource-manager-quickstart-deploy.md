---
title: "Erőforrások üzembe helyezése az Azure-ban | Microsoft Docs"
description: "Az Azure PowerShell vagy Azure CLI használatával helyezhet üzembe erőforrásokat az Azure-ban. Az erőforrások egy Resource Manager-sablonban vannak meghatározva."
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
ms.openlocfilehash: 19d5ec337a18b1a159de05ed611b2ccd0c15c592
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-resources-to-azure"></a><span data-ttu-id="fdc8d-104">Erőforrások üzembe helyezése az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="fdc8d-104">Deploy resources to Azure</span></span>

<span data-ttu-id="fdc8d-105">Ez a témakör bemutatja, hogyan helyezhet üzembe erőforrásokat az Azure-előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-105">This topic shows how to deploy resources to your Azure subscription.</span></span> <span data-ttu-id="fdc8d-106">Az Azure PowerShell vagy az Azure CLI használatával helyezhet üzembe a megoldása infrastruktúráját meghatározó Resource Manager-sablont.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-106">You can use either Azure PowerShell or Azure CLI to deploy a Resource Manager template that defines the infrastructure for your solution.</span></span>

<span data-ttu-id="fdc8d-107">A Resource Manager alapelveinek áttekintése: [Az Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fdc8d-107">For an introduction to concepts of Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="steps-for-deployment"></a><span data-ttu-id="fdc8d-108">Az üzembe helyezés lépései</span><span class="sxs-lookup"><span data-stu-id="fdc8d-108">Steps for deployment</span></span>

<span data-ttu-id="fdc8d-109">Ez a témakör feltételezi, hogy a témakör [példa tárolósablonját](#example-storage-template) helyezi üzembe.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-109">This topic assumes you are deploying the [example storage template](#example-storage-template) in this topic.</span></span> <span data-ttu-id="fdc8d-110">Használhat más sablont, de az átadott paraméterek különböznek a témakörben láthatóktól.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-110">You can use a different template, but the parameters you pass are different than what is shown in this topic.</span></span>

<span data-ttu-id="fdc8d-111">A sablon létrehozása után a sablon üzembe helyezésének általános lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-111">After creating a template, the general steps for deploying your template are:</span></span>

1. <span data-ttu-id="fdc8d-112">Bejelentkezés a fiókjába</span><span class="sxs-lookup"><span data-stu-id="fdc8d-112">Log in to your account</span></span>
2. <span data-ttu-id="fdc8d-113">A használni kívánt előfizetés kiválasztása (csak akkor szükséges, ha több előfizetése van, és nem az alapértelmezett előfizetést szeretné használni)</span><span class="sxs-lookup"><span data-stu-id="fdc8d-113">Select the subscription to use (only necessary if you have multiple subscriptions, and you want to use one that is not the default subscription)</span></span>
3. <span data-ttu-id="fdc8d-114">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="fdc8d-114">Create a resource group</span></span>
4. <span data-ttu-id="fdc8d-115">A sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="fdc8d-115">Deploy the template</span></span>
5. <span data-ttu-id="fdc8d-116">Az üzemelő példány állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="fdc8d-116">Check your deployment status</span></span>

<span data-ttu-id="fdc8d-117">A következő szakaszok bemutatják, hogyan végezheti el ezeket a lépéseket a [PowerShell](#powershell) vagy az [Azure CLI](#azure-cli) használatával.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-117">The following sections show how to perform those steps with [PowerShell](#powershell) or [Azure CLI](#azure-cli).</span></span>

## <a name="powershell"></a><span data-ttu-id="fdc8d-118">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdc8d-118">PowerShell</span></span>

1. <span data-ttu-id="fdc8d-119">Az Azure PowerShell telepítése: [Az Azure PowerShell-parancsmagok használatának első lépései](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fdc8d-119">To install Azure PowerShell, see [Get started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>

2. <span data-ttu-id="fdc8d-120">Az üzemelő példány használatának gyors megkezdése érdekében használja a következő parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-120">To quickly get started with deployment, use the following cmdlets:</span></span>

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  <span data-ttu-id="fdc8d-121">A `Set-AzureRmContext` parancsmagra csak akkor van szükség, ha nem az alapértelmezett előfizetést szeretné használni.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-121">The `Set-AzureRmContext` cmdlet is only needed if you want to use a subscription other than your default subscription.</span></span> <span data-ttu-id="fdc8d-122">Az összes előfizetés és a hozzájuk tartozó azonosítóik megtekintéséhez használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-122">To see all your subscriptions and their IDs, use:</span></span>

  ```powershell
  Get-AzureRmSubscription
  ```

3. <span data-ttu-id="fdc8d-123">Az üzembe helyezés eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-123">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="fdc8d-124">Amikor befejeződik, a következőhöz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-124">When it finishes, you see a message similar to:</span></span>

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. <span data-ttu-id="fdc8d-125">Ha ellenőrizni szeretné, hogy az erőforráscsoportja és a tárfiókja üzembe van-e helyezve az előfizetésben, használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-125">To see that your resource group and storage account were deployed to your subscription, use:</span></span>

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. <span data-ttu-id="fdc8d-126">Sablon üzembe helyezésekor a sablon paramétereit megadhatja PowerShell-paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-126">You can specify template parameters as PowerShell parameters when deploying a template.</span></span> <span data-ttu-id="fdc8d-127">A korábbi példában nem szerepeltek sablonparaméterek, így a rendszer a sablon alapértelmezett értékeit használta.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-127">The earlier example did not include any template parameters, so the default values in the template were used.</span></span> <span data-ttu-id="fdc8d-128">Másik tárfiók üzembe helyezéséhez, valamint a tárolónév előtagjához és a tárfiók SKU-jához tartozó paraméterértékek megadásához használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-128">To deploy another storage account, and provide parameter values for the storage name prefix and the storage account SKU, use:</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  <span data-ttu-id="fdc8d-129">Most két tárfiókja van az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-129">You now have two storage accounts in your resource group.</span></span> 

## <a name="azure-cli"></a><span data-ttu-id="fdc8d-130">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fdc8d-130">Azure CLI</span></span>

1. <span data-ttu-id="fdc8d-131">Az Azure CLI telepítése: [Az Azure CLI 2.0 telepítése](/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="fdc8d-131">To install Azure CLI, see [Install Azure CLI 2.0](/cli/azure/install-az-cli2).</span></span>

2. <span data-ttu-id="fdc8d-132">Az üzembe helyezés gyors megkezdéséhez használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-132">To quickly get started with deployment, use the following commands:</span></span>

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  <span data-ttu-id="fdc8d-133">A `az account set` parancsra csak akkor van szükség, ha nem az alapértelmezett előfizetést szeretné használni.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-133">The `az account set` command is only needed if you want to use a subscription other than your default subscription.</span></span> <span data-ttu-id="fdc8d-134">Az összes előfizetés és a hozzájuk tartozó azonosítóik megtekintéséhez használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-134">To see all your subscriptions and their IDs, use:</span></span>

  ```azurecli
  az account list
  ```

3. <span data-ttu-id="fdc8d-135">Az üzembe helyezés eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-135">The deployment can take a few minutes to complete.</span></span> <span data-ttu-id="fdc8d-136">Amikor befejeződik, a következőhöz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-136">When it finishes, you see a message similar to:</span></span>

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. <span data-ttu-id="fdc8d-137">Ha ellenőrizni szeretné, hogy az erőforráscsoportja és a tárfiókja üzembe van-e helyezve az előfizetésben, használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-137">To see that your resource group and storage account were deployed to your subscription, use:</span></span>

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. <span data-ttu-id="fdc8d-138">Sablon üzembe helyezésekor a sablon paramétereit megadhatja PowerShell-paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-138">You can specify template parameters as PowerShell parameters when deploying a template.</span></span> <span data-ttu-id="fdc8d-139">A korábbi példában nem szerepeltek sablonparaméterek, így a rendszer a sablon alapértelmezett értékeit használta.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-139">The earlier example did not include any template parameters, so the default values in the template were used.</span></span> <span data-ttu-id="fdc8d-140">Másik tárfiók üzembe helyezéséhez, valamint a tárolónév előtagjához és a tárfiók SKU-jához tartozó paraméterértékek megadásához használja a következőt:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-140">To deploy another storage account, and provide parameter values for the storage name prefix and the storage account SKU, use:</span></span>

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  <span data-ttu-id="fdc8d-141">Most két tárfiókja van az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="fdc8d-141">You now have two storage accounts in your resource group.</span></span> 

## <a name="example-storage-template"></a><span data-ttu-id="fdc8d-142">Példa tárolósablon</span><span class="sxs-lookup"><span data-stu-id="fdc8d-142">Example storage template</span></span>

<span data-ttu-id="fdc8d-143">A következő példasablonnal üzembe helyezhet egy tárfiókot az előfizetésben:</span><span class="sxs-lookup"><span data-stu-id="fdc8d-143">Use the following example template to deploy a storage account to your subscription:</span></span>

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
        "description": "The value to use for starting the storage account name."
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
        "description": "The type of replication to use for the storage account."
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

## <a name="next-steps"></a><span data-ttu-id="fdc8d-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdc8d-144">Next steps</span></span>

* <span data-ttu-id="fdc8d-145">Részletes információ a sablonok PowerShell használatával végzett üzembe helyezéséről: [Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure PowerShell-lel](/azure/azure-resource-manager/resource-group-template-deploy).</span><span class="sxs-lookup"><span data-stu-id="fdc8d-145">For detailed information about using PowerShell to deploy templates, see [Deploy resources with Resource Manager templates and Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy).</span></span>
* <span data-ttu-id="fdc8d-146">Részletes információ a sablonok Azure CLI-vel végzett üzembe helyezéséről: [Erőforrások üzembe helyezése Resource Manager-sablonokkal és az Azure CLI-vel](/azure/azure-resource-manager/resource-group-template-deploy-cli).</span><span class="sxs-lookup"><span data-stu-id="fdc8d-146">For detailed information about using Azure CLI to deploy templates, see [Deploy resources with Resource Manager templates and Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli).</span></span>



