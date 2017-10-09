---
title: "az Azure Resource Manager Machine Learning-munkaterület aaaDeploy |} Microsoft Docs"
description: "Hogyan toodeploy az Azure Machine Learning Azure Resource Manager sablonnal munkaterület"
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
ms.openlocfilehash: 308959825bcbd670f6ce9b6dc381be767f172357
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a><span data-ttu-id="eb7ec-103">Machine Learning-munkaterület üzembe helyezése az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="eb7ec-103">Deploy Machine Learning Workspace Using Azure Resource Manager</span></span>
## <a name="introduction"></a><span data-ttu-id="eb7ec-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="eb7ec-104">Introduction</span></span>
<span data-ttu-id="eb7ec-105">A központi telepítési sablont menti, Azure Resource Manager használatával adjon meg egy méretezhető módon toodeploy idő összekapcsolt összetevők az érvényesítési és újrapróbálkozási mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-105">Using an Azure Resource Manager deployment template saves you time by giving you a scalable way toodeploy interconnected components with a validation and retry mechanism.</span></span> <span data-ttu-id="eb7ec-106">Azure Machine Learning munkaterületek mentése tooset, például szüksége toofirst konfigurálása az Azure-tárfiók, és üzembe helyezhesse a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-106">tooset up Azure Machine Learning Workspaces, for example, you need toofirst configure an Azure storage account and then deploy your workspace.</span></span> <span data-ttu-id="eb7ec-107">Tegyük fel, így manuálisan munkaterületek több száz.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-107">Imagine doing this manually for hundreds of workspaces.</span></span> <span data-ttu-id="eb7ec-108">Egyszerűbb alternatív egy Azure Resource Manager sablon toodeploy egy Azure Machine Learning munkaterülettel toouse és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-108">An easier alternative is toouse an Azure Resource Manager template toodeploy an Azure Machine Learning Workspace and all its dependencies.</span></span> <span data-ttu-id="eb7ec-109">Ez a cikk végigvezeti a részletes folyamat.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-109">This article takes you through this process step-by-step.</span></span> <span data-ttu-id="eb7ec-110">Az Azure Resource Manager kiváló, áttekintés: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eb7ec-110">For a great overview of Azure Resource Manager, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="step-by-step-create-a-machine-learning-workspace"></a><span data-ttu-id="eb7ec-111">Részletes útmutató: a Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb7ec-111">Step-by-step: create a Machine Learning Workspace</span></span>
<span data-ttu-id="eb7ec-112">Azt fogja hozzon létre egy Azure-erőforráscsoportot, majd a központi telepítése egy új Azure-tárfiókot és egy új Azure Machine Learning munkaterülettel Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-112">We will create an Azure resource group, then deploy a new Azure storage account and a new Azure Machine Learning Workspace using a Resource Manager template.</span></span> <span data-ttu-id="eb7ec-113">Hello központi telepítés befejezése után a rendszer (elsődleges kulcs hello hello workspaceID és hello URL-cím toohello munkaterület) létrehozott hello munkaterületekkel kapcsolatos fontos információkat nyomtatásához.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-113">Once hello deployment is complete, we will print out important information about hello workspaces that were created (hello primary key, hello workspaceID, and hello URL toohello workspace).</span></span>

### <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="eb7ec-114">Az Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb7ec-114">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="eb7ec-115">A Machine Learning-munkaterület szükséges egy az Azure storage-fiók toostore hello társított dataset tooit.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-115">A Machine Learning Workspace requires an Azure storage account toostore hello dataset linked tooit.</span></span>
<span data-ttu-id="eb7ec-116">hello következő sablon hello nevét használja hello erőforráscsoport toogenerate hello tárolási fiók neve és hello munkaterület nevét.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-116">hello following template uses hello name of hello resource group toogenerate hello storage account name and hello workspace name.</span></span>  <span data-ttu-id="eb7ec-117">Is használja a tárfiók neve hello tulajdonságként hello munkaterület létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-117">It also uses hello storage account name as a property when creating hello workspace.</span></span>

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
<span data-ttu-id="eb7ec-118">Ez a sablon mentése c:\temp\ mlworkspace.json fájlt.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-118">Save this template as mlworkspace.json file under c:\temp\.</span></span>

### <a name="deploy-hello-resource-group-based-on-hello-template"></a><span data-ttu-id="eb7ec-119">Hello sablonon alapuló hello erőforrás-csoport központi telepítése</span><span class="sxs-lookup"><span data-stu-id="eb7ec-119">Deploy hello resource group, based on hello template</span></span>
* <span data-ttu-id="eb7ec-120">Nyissa meg a PowerShell</span><span class="sxs-lookup"><span data-stu-id="eb7ec-120">Open PowerShell</span></span>
* <span data-ttu-id="eb7ec-121">Az Azure Resource Manager és az Azure Service Management-modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="eb7ec-121">Install modules for Azure Resource Manager and Azure Service Management</span></span>  

```
# Install hello Azure Resource Manager modules from hello PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install hello Azure Service Management modules from hello PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   <span data-ttu-id="eb7ec-122">Ezeket a lépéseket töltse le és telepítse hello modulok szükséges toocomplete hello fennmaradó lépéseit.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-122">These steps download and install hello modules necessary toocomplete hello remaining steps.</span></span> <span data-ttu-id="eb7ec-123">Csak egyszer történik hello környezetben, ahol hello PowerShell-parancsok végrehajtása toobe kell.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-123">This only needs toobe done once in hello environment where you are executing hello PowerShell commands.</span></span>   

* <span data-ttu-id="eb7ec-124">TooAzure hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="eb7ec-124">Authenticate tooAzure</span></span>  

```
# Authenticate (enter your credentials in hello pop-up window)
Add-AzureRmAccount
```
<span data-ttu-id="eb7ec-125">Ezt a lépést minden munkamenet ismétlődő toobe kell.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-125">This step needs toobe repeated for each session.</span></span> <span data-ttu-id="eb7ec-126">Ha hitelesítése megtörtént, az előfizetési adatok üzenetnek kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-126">Once authenticated, your subscription information should be displayed.</span></span>

![Azure-fiók][1]

<span data-ttu-id="eb7ec-128">Most, hogy hozzáférési tooAzure, létrehozhatjuk hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-128">Now that we have access tooAzure, we can create hello resource group.</span></span>

* <span data-ttu-id="eb7ec-129">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="eb7ec-129">Create a resource group</span></span>

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

<span data-ttu-id="eb7ec-130">Ellenőrizze, hogy hello erőforráscsoport megfelelően lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-130">Verify that hello resource group is correctly provisioned.</span></span> <span data-ttu-id="eb7ec-131">**ProvisioningState** kell lennie "sikeresen befejeződött."</span><span class="sxs-lookup"><span data-stu-id="eb7ec-131">**ProvisioningState** should be “Succeeded.”</span></span>
<span data-ttu-id="eb7ec-132">hello sablon toogenerate hello tárfióknév hello erőforráscsoport nevét használja.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-132">hello resource group name is used by hello template toogenerate hello storage account name.</span></span> <span data-ttu-id="eb7ec-133">hello tárfiók neve 3 – 24 karakter hosszúságúnak kell és számokat és kisbetűket tartalmazhatnak csak.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-133">hello storage account name must be between 3 and 24 characters in length and use numbers and lower-case letters only.</span></span>

![Erőforráscsoport][2]

* <span data-ttu-id="eb7ec-135">Egy új Machine Learning munkaterületének használatával hello erőforrás csoport központi telepítése, központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-135">Using hello resource group deployment, deploy a new Machine Learning Workspace.</span></span>

```
# Create a Resource Group, TemplateFile is hello location of hello JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

<span data-ttu-id="eb7ec-136">Ha hello telepítés befejeződött, egyszerű tooaccess tulajdonságok telepített hello munkaterület.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-136">Once hello deployment is completed, it is straightforward tooaccess properties of hello workspace you deployed.</span></span> <span data-ttu-id="eb7ec-137">Például az elsődleges kulcs jogkivonata hello végezheti el.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-137">For example, you can access hello Primary Key Token.</span></span>

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

<span data-ttu-id="eb7ec-138">Egy másik módja tooretrieve jogkivonatok meglévő munkaterület toouse hello Invoke-AzureRmResourceAction parancs.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-138">Another way tooretrieve tokens of existing workspace is toouse hello Invoke-AzureRmResourceAction command.</span></span> <span data-ttu-id="eb7ec-139">Például listázhatja hello elsődleges és másodlagos jogkivonatok összes munkaterületet.</span><span class="sxs-lookup"><span data-stu-id="eb7ec-139">For example, you can list hello primary and secondary tokens of all workspaces.</span></span>

```  
# List hello primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
<span data-ttu-id="eb7ec-140">Hello munkaterület üzembe helyezése után is automatizálhatja a hello segítségével számos Azure Machine Learning Studio feladat [Azure Machine Learning PowerShell-modul](http://aka.ms/amlps).</span><span class="sxs-lookup"><span data-stu-id="eb7ec-140">After hello workspace is provisioned, you can also automate many Azure Machine Learning Studio tasks using hello [PowerShell Module for Azure Machine Learning](http://aka.ms/amlps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="eb7ec-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb7ec-141">Next Steps</span></span>
* <span data-ttu-id="eb7ec-142">További információ [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="eb7ec-142">Learn more about [authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="eb7ec-143">Tekintse meg a következő hello [Azure gyors üzembe helyezés sablonok tárházba](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="eb7ec-143">Have a look at hello [Azure Quickstart Templates Repository](https://github.com/Azure/azure-quickstart-templates).</span></span> 
* <span data-ttu-id="eb7ec-144">Ezt a videót kapcsolatos [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span><span class="sxs-lookup"><span data-stu-id="eb7ec-144">Watch this video about [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).</span></span> 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
