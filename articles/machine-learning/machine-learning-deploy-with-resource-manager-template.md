---
title: "Az Azure Resource Manager Machine Learning-munkaterület központi telepítése |} Microsoft Docs"
description: "A munkaterület telepítése az Azure Machine Learning Azure Resource Manager-sablonnal"
services: machine-learning
documentationcenter: 
author: ahgyger
manager: haining
editor: garye
ms.assetid: 4955ac4d-ff99-4908-aa27-69b6bfcc8e85
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/15/2017
ms.author: ahgyger
ms.openlocfilehash: 9e37780428b0867da63987ec4f7f843a8abeb907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a><span data-ttu-id="012fb-103">Machine Learning-munkaterület üzembe helyezése az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="012fb-103">Deploy Machine Learning Workspace Using Azure Resource Manager</span></span>
## <a name="introduction"></a><span data-ttu-id="012fb-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="012fb-104">Introduction</span></span>
<span data-ttu-id="012fb-105">A telepítési sablonnak adjon meg egy méretezhető módja, időt takaríthat meg Azure Resource Manager összekapcsolt érvényesség-összetevők telepítéséhez, majd próbálja megismételni a mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="012fb-105">Using an Azure Resource Manager deployment template saves you time by giving you a scalable way to deploy interconnected components with a validation and retry mechanism.</span></span> <span data-ttu-id="012fb-106">Azure Machine Learning munkaterületek beállítása, például szüksége a munkaterület majd alkalmaznia kell konfigurálnia az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="012fb-106">To set up Azure Machine Learning Workspaces, for example, you need to first configure an Azure storage account and then deploy your workspace.</span></span> <span data-ttu-id="012fb-107">Tegyük fel, így manuálisan munkaterületek több száz.</span><span class="sxs-lookup"><span data-stu-id="012fb-107">Imagine doing this manually for hundreds of workspaces.</span></span> <span data-ttu-id="012fb-108">Megkönnyíti a másik lehetőség az Azure Resource Manager-sablonok segítségével központi telepítése egy Azure Machine Learning munkaterülettel és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="012fb-108">An easier alternative is to use an Azure Resource Manager template to deploy an Azure Machine Learning Workspace and all its dependencies.</span></span> <span data-ttu-id="012fb-109">Ez a cikk végigvezeti a részletes folyamat.</span><span class="sxs-lookup"><span data-stu-id="012fb-109">This article takes you through this process step-by-step.</span></span> <span data-ttu-id="012fb-110">Az Azure Resource Manager kiváló, áttekintés: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="012fb-110">For a great overview of Azure Resource Manager, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="step-by-step-create-a-machine-learning-workspace"></a><span data-ttu-id="012fb-111">Részletes útmutató: a Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="012fb-111">Step-by-step: create a Machine Learning Workspace</span></span>
<span data-ttu-id="012fb-112">Azt fogja hozzon létre egy Azure-erőforráscsoportot, majd a központi telepítése egy új Azure-tárfiókot és egy új Azure Machine Learning munkaterülettel Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="012fb-112">We will create an Azure resource group, then deploy a new Azure storage account and a new Azure Machine Learning Workspace using a Resource Manager template.</span></span> <span data-ttu-id="012fb-113">Ha a telepítés befejeződött, azt fogja nyomtassa ki a munkaterületek (az elsődleges kulcs, a workspaceID és az URL-cím a munkaterületre) létrehozott fontos információkat.</span><span class="sxs-lookup"><span data-stu-id="012fb-113">Once the deployment is complete, we will print out important information about the workspaces that were created (the primary key, the workspaceID, and the URL to the workspace).</span></span>

### <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="012fb-114">Az Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="012fb-114">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="012fb-115">A Machine Learning-munkaterület kapcsolni az adatkészlet tárolásához Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="012fb-115">A Machine Learning Workspace requires an Azure storage account to store the dataset linked to it.</span></span>
<span data-ttu-id="012fb-116">Az alábbi sablont létrehozni a tárfiók nevét az erőforráscsoport nevét és a munkaterület nevét használja.</span><span class="sxs-lookup"><span data-stu-id="012fb-116">The following template uses the name of the resource group to generate the storage account name and the workspace name.</span></span>  <span data-ttu-id="012fb-117">Azt is használja a tárfiók neve tulajdonságként létrehozásakor a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="012fb-117">It also uses the storage account name as a property when creating the workspace.</span></span>

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
<span data-ttu-id="012fb-118">Ez a sablon mentése c:\temp\ mlworkspace.json fájlt.</span><span class="sxs-lookup"><span data-stu-id="012fb-118">Save this template as mlworkspace.json file under c:\temp\.</span></span>

### <a name="deploy-the-resource-group-based-on-the-template"></a><span data-ttu-id="012fb-119">Az erőforráscsoport a sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="012fb-119">Deploy the resource group, based on the template</span></span>
* <span data-ttu-id="012fb-120">Nyissa meg a PowerShell</span><span class="sxs-lookup"><span data-stu-id="012fb-120">Open PowerShell</span></span>
* <span data-ttu-id="012fb-121">Az Azure Resource Manager és az Azure Service Management-modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="012fb-121">Install modules for Azure Resource Manager and Azure Service Management</span></span>  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   <span data-ttu-id="012fb-122">Ezeket a lépéseket töltse le és telepítse a további lépéseket befejezéséhez szükséges modulokat.</span><span class="sxs-lookup"><span data-stu-id="012fb-122">These steps download and install the modules necessary to complete the remaining steps.</span></span> <span data-ttu-id="012fb-123">Ez csak azért van szükség, egyszer a környezetben, ahol a PowerShell-parancsok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="012fb-123">This only needs to be done once in the environment where you are executing the PowerShell commands.</span></span>   

* <span data-ttu-id="012fb-124">Hitelesítés az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="012fb-124">Authenticate to Azure</span></span>  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
<span data-ttu-id="012fb-125">Ebben a lépésben meg kell ismételni, mindegyik munkamenethez.</span><span class="sxs-lookup"><span data-stu-id="012fb-125">This step needs to be repeated for each session.</span></span> <span data-ttu-id="012fb-126">Ha hitelesítése megtörtént, az előfizetési adatok üzenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="012fb-126">Once authenticated, your subscription information should be displayed.</span></span>

![Azure-fiók][1]

<span data-ttu-id="012fb-128">Most, hogy Azure-hozzáférést, vannak, létrehozható az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="012fb-128">Now that we have access to Azure, we can create the resource group.</span></span>

* <span data-ttu-id="012fb-129">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="012fb-129">Create a resource group</span></span>

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

<span data-ttu-id="012fb-130">Győződjön meg arról, hogy az erőforráscsoport megfelelően lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="012fb-130">Verify that the resource group is correctly provisioned.</span></span> <span data-ttu-id="012fb-131">**ProvisioningState** kell lennie "sikeresen befejeződött."</span><span class="sxs-lookup"><span data-stu-id="012fb-131">**ProvisioningState** should be “Succeeded.”</span></span>
<span data-ttu-id="012fb-132">Az erőforráscsoport neve a sablon a tárfiók neve használt.</span><span class="sxs-lookup"><span data-stu-id="012fb-132">The resource group name is used by the template to generate the storage account name.</span></span> <span data-ttu-id="012fb-133">A tárfiók nevét kell 3 és 24 karakter hosszúságúnak és kell használnia csak számokat és kisbetűket tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="012fb-133">The storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

![Erőforráscsoport][2]

* <span data-ttu-id="012fb-135">Egy új Machine Learning-munkaterület használata az erőforrás-csoport központi telepítése, központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="012fb-135">Using the resource group deployment, deploy a new Machine Learning Workspace.</span></span>

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

<span data-ttu-id="012fb-136">Ha a telepítés befejeződött, akkor magától értetődő telepítette a munkaterület tulajdonságainak hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="012fb-136">Once the deployment is completed, it is straightforward to access properties of the workspace you deployed.</span></span> <span data-ttu-id="012fb-137">Például végezheti el az elsődleges kulcs Token.</span><span class="sxs-lookup"><span data-stu-id="012fb-137">For example, you can access the Primary Key Token.</span></span>

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

<span data-ttu-id="012fb-138">Egy másik meglévő munkaterület jogkivonatok beolvasása módja az Invoke-AzureRmResourceAction parancs használata.</span><span class="sxs-lookup"><span data-stu-id="012fb-138">Another way to retrieve tokens of existing workspace is to use the Invoke-AzureRmResourceAction command.</span></span> <span data-ttu-id="012fb-139">Például listázhatja az összes munkaterületek elsődleges és másodlagos jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="012fb-139">For example, you can list the primary and secondary tokens of all workspaces.</span></span>

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
<span data-ttu-id="012fb-140">A munkaterület üzembe helyezése után is automatizálhatja a sok Azure Machine Learning Studio feladatok a [Azure Machine Learning PowerShell-modul](http://aka.ms/amlps).</span><span class="sxs-lookup"><span data-stu-id="012fb-140">After the workspace is provisioned, you can also automate many Azure Machine Learning Studio tasks using the [PowerShell Module for Azure Machine Learning](http://aka.ms/amlps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="012fb-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="012fb-141">Next Steps</span></span>
* <span data-ttu-id="012fb-142">További információ [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="012fb-142">Learn more about [authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="012fb-143">Tekintse meg a következő a [Azure gyors üzembe helyezés sablonok tárházba](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="012fb-143">Have a look at the [Azure Quickstart Templates Repository](https://github.com/Azure/azure-quickstart-templates).</span></span> 
* <span data-ttu-id="012fb-144">Ezt a videót kapcsolatos [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span><span class="sxs-lookup"><span data-stu-id="012fb-144">Watch this video about [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span></span> 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
