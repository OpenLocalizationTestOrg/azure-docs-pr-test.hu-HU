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
# <a name="deploy-azure-resources-toomore-than-one-resource-group"></a><span data-ttu-id="c8ad7-103">Azure-erőforrások toomore, mint egy erőforráscsoport telepítése</span><span class="sxs-lookup"><span data-stu-id="c8ad7-103">Deploy Azure resources toomore than one resource group</span></span>

<span data-ttu-id="c8ad7-104">Általában minden hello erőforrások telepít a sablon tooa egyetlen erőforráscsoportként működnek.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-104">Typically, you deploy all hello resources in your template tooa single resource group.</span></span> <span data-ttu-id="c8ad7-105">Vannak azonban forgatókönyvek, ahol szeretné, hogy toodeploy erőforráscsoport együtt, de helyezze el őket a különböző erőforráscsoportokra.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-105">However, there are scenarios where you want toodeploy a set of resources together but place them in different resource groups.</span></span> <span data-ttu-id="c8ad7-106">Például érdemes lehet toodeploy hello tartalék virtuális gép az Azure Site Recovery tooa külön erőforráscsoportot és helyet.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-106">For example, you may want toodeploy hello backup virtual machine for Azure Site Recovery tooa separate resource group and location.</span></span> <span data-ttu-id="c8ad7-107">Erőforrás-kezelő lehetővé teszi beágyazott toouse sablonok tootarget eltérő erőforráscsoportokban hello hello szülő sablon használt erőforráscsoport-nál.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-107">Resource Manager enables you toouse nested templates tootarget different resource groups than hello resource group used for hello parent template.</span></span>

<span data-ttu-id="c8ad7-108">hello erőforráscsoport hello életciklus tároló hello alkalmazás és az erőforrások gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-108">hello resource group is hello lifecycle container for hello application and its collection of resources.</span></span> <span data-ttu-id="c8ad7-109">Hello erőforráscsoport kívül hello sablon létrehozásához, és adja meg a hello erőforrás csoport tootarget üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-109">You create hello resource group outside of hello template, and specify hello resource group tootarget during deployment.</span></span> <span data-ttu-id="c8ad7-110">Egy bevezető tooresource csoportok, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c8ad7-110">For an introduction tooresource groups, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>

## <a name="example-template"></a><span data-ttu-id="c8ad7-111">Példa sablon</span><span class="sxs-lookup"><span data-stu-id="c8ad7-111">Example template</span></span>

<span data-ttu-id="c8ad7-112">tootarget egy másik erőforráscsoportban, kell használnia egy beágyazott vagy csatolt sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-112">tootarget a different resource, you must use a nested or linked template during deployment.</span></span> <span data-ttu-id="c8ad7-113">Hello `Microsoft.Resources/deployments` erőforrástípust biztosít egy `resourceGroup` paraméter, amely lehetővé teszi, hogy egy másik erőforráscsoportban található a hello toospecify beágyazott központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-113">hello `Microsoft.Resources/deployments` resource type provides a `resourceGroup` parameter that enables you toospecify a different resource group for hello nested deployment.</span></span> <span data-ttu-id="c8ad7-114">Az összes hello erőforráscsoport léteznie kell a hello központi telepítés futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-114">All hello resource groups must exist before running hello deployment.</span></span> <span data-ttu-id="c8ad7-115">hello következő példa telepíti két storage-fiókok – egy a telepítés során megadott hello erőforráscsoportot és egy-egy nevű erőforráscsoport `crossResourceGroupDeployment`:</span><span class="sxs-lookup"><span data-stu-id="c8ad7-115">hello following example deploys two storage accounts - one in hello resource group specified during deployment, and one in a resource group named `crossResourceGroupDeployment`:</span></span>

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

<span data-ttu-id="c8ad7-116">Ha `resourceGroup` toohello nevét, amely nem található erőforráscsoport, hello központi telepítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-116">If you set `resourceGroup` toohello name of a resource group that does not exist, hello deployment fails.</span></span> <span data-ttu-id="c8ad7-117">Ha nem ad meg értéket `resourceGroup`, erőforrás-kezelő hello szülő erőforráscsoport használja.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-117">If you do not provide a value for `resourceGroup`, Resource Manager uses hello parent resource group.</span></span>  

## <a name="deploy-hello-template"></a><span data-ttu-id="c8ad7-118">Hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="c8ad7-118">Deploy hello template</span></span>

<span data-ttu-id="c8ad7-119">toodeploy hello példa sablon használható hello portál, az Azure PowerShell vagy az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-119">toodeploy hello example template, you can use hello portal, Azure PowerShell, or Azure CLI.</span></span> <span data-ttu-id="c8ad7-120">Az Azure PowerShell vagy Azure CLI-t előfordulhat, hogy 2017, vagy később kiadási kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-120">For Azure PowerShell or Azure CLI, you must use a release from May 2017 or later.</span></span> <span data-ttu-id="c8ad7-121">hello példák azt feltételezik, hogy mentett hello sablon helyileg nevű fájl **crossrgdeployment.json**.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-121">hello examples assume you have saved hello template locally as a file named **crossrgdeployment.json**.</span></span>

<span data-ttu-id="c8ad7-122">A PowerShell környezethez:</span><span class="sxs-lookup"><span data-stu-id="c8ad7-122">For PowerShell:</span></span>

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

<span data-ttu-id="c8ad7-123">Az Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="c8ad7-123">For Azure CLI:</span></span>

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

<span data-ttu-id="c8ad7-124">Központi telepítés befejezése után két erőforráscsoport láthatja.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-124">After deployment completes, you see two resource groups.</span></span> <span data-ttu-id="c8ad7-125">Mindegyik erőforráscsoport egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-125">Each resource group contains a storage account.</span></span>

## <a name="use-resourcegroup-function"></a><span data-ttu-id="c8ad7-126">ResourceGroup() funkcióval</span><span class="sxs-lookup"><span data-stu-id="c8ad7-126">Use resourceGroup() function</span></span>

<span data-ttu-id="c8ad7-127">A tartományok közötti erőforrás-csoport központi telepítések, hello [resouceGroup() függvény](resource-group-template-functions-resource.md#resourcegroup) oldja fel a rendszer menübeállításoktól függően hogyan hello beágyazott sablon határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-127">For cross resource group deployments, hello [resouceGroup() function](resource-group-template-functions-resource.md#resourcegroup) resolves differently based on how you specify hello nested template.</span></span> 

<span data-ttu-id="c8ad7-128">Ha egy sablon belül egy másik sablon beágyazásához resouceGroup() hello beágyazott sablonban toohello szülő erőforráscsoport szünteti meg.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-128">If you embed one template within another template, resouceGroup() in hello nested template resolves toohello parent resource group.</span></span> <span data-ttu-id="c8ad7-129">Egy beágyazott sablon hello a következő formátumot használja:</span><span class="sxs-lookup"><span data-stu-id="c8ad7-129">An embedded template uses hello following format:</span></span>

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

<span data-ttu-id="c8ad7-130">Ha külön sablon tooa, resouceGroup() hello csatolt sablonban toohello beágyazott erőforráscsoport oldja fel.</span><span class="sxs-lookup"><span data-stu-id="c8ad7-130">If you link tooa separate template, resouceGroup() in hello linked template resolves toohello nested resource group.</span></span> <span data-ttu-id="c8ad7-131">Csatolt sablon hello a következő formátumot használja:</span><span class="sxs-lookup"><span data-stu-id="c8ad7-131">A linked template uses hello following format:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c8ad7-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8ad7-132">Next steps</span></span>

* <span data-ttu-id="c8ad7-133">Hogyan toodefine paramétereket a sablonban: toounderstand [hello struktúra és az Azure Resource Manager-sablonok szintaxisát](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="c8ad7-133">toounderstand how toodefine parameters in your template, see [Understand hello structure and syntax of Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="c8ad7-134">Tippek az általános telepítési hibák feloldására, lásd: [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="c8ad7-134">For tips on resolving common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="c8ad7-135">A sablont, amely a SAS-jogkivonat szükséges, központi telepítésével kapcsolatos információkért lásd: [telepítés titkos sablont a SAS-jogkivonat](resource-manager-powershell-sas-token.md).</span><span class="sxs-lookup"><span data-stu-id="c8ad7-135">For information about deploying a template that requires a SAS token, see [Deploy private template with SAS token](resource-manager-powershell-sas-token.md).</span></span>
