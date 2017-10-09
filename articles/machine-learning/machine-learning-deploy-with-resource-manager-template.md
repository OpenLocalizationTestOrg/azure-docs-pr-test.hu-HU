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
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Machine Learning-munkaterület üzembe helyezése az Azure Resource Manager használatával
## <a name="introduction"></a>Bevezetés
A központi telepítési sablont menti, Azure Resource Manager használatával adjon meg egy méretezhető módon toodeploy idő összekapcsolt összetevők az érvényesítési és újrapróbálkozási mechanizmus. Azure Machine Learning munkaterületek mentése tooset, például szüksége toofirst konfigurálása az Azure-tárfiók, és üzembe helyezhesse a munkaterületen. Tegyük fel, így manuálisan munkaterületek több száz. Egyszerűbb alternatív egy Azure Resource Manager sablon toodeploy egy Azure Machine Learning munkaterülettel toouse és annak függőségeit. Ez a cikk végigvezeti a részletes folyamat. Az Azure Resource Manager kiváló, áttekintés: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Részletes útmutató: a Machine Learning-munkaterület létrehozása
Azt fogja hozzon létre egy Azure-erőforráscsoportot, majd a központi telepítése egy új Azure-tárfiókot és egy új Azure Machine Learning munkaterülettel Resource Manager-sablon használatával. Hello központi telepítés befejezése után a rendszer (elsődleges kulcs hello hello workspaceID és hello URL-cím toohello munkaterület) létrehozott hello munkaterületekkel kapcsolatos fontos információkat nyomtatásához.

### <a name="create-an-azure-resource-manager-template"></a>Az Azure Resource Manager-sablon létrehozása
A Machine Learning-munkaterület szükséges egy az Azure storage-fiók toostore hello társított dataset tooit.
hello következő sablon hello nevét használja hello erőforráscsoport toogenerate hello tárolási fiók neve és hello munkaterület nevét.  Is használja a tárfiók neve hello tulajdonságként hello munkaterület létrehozása során.

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
Ez a sablon mentése c:\temp\ mlworkspace.json fájlt.

### <a name="deploy-hello-resource-group-based-on-hello-template"></a>Hello sablonon alapuló hello erőforrás-csoport központi telepítése
* Nyissa meg a PowerShell
* Az Azure Resource Manager és az Azure Service Management-modulok telepítése  

```
# Install hello Azure Resource Manager modules from hello PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install hello Azure Service Management modules from hello PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Ezeket a lépéseket töltse le és telepítse hello modulok szükséges toocomplete hello fennmaradó lépéseit. Csak egyszer történik hello környezetben, ahol hello PowerShell-parancsok végrehajtása toobe kell.   

* TooAzure hitelesítéséhez  

```
# Authenticate (enter your credentials in hello pop-up window)
Add-AzureRmAccount
```
Ezt a lépést minden munkamenet ismétlődő toobe kell. Ha hitelesítése megtörtént, az előfizetési adatok üzenetnek kell megjelennie.

![Azure-fiók][1]

Most, hogy hozzáférési tooAzure, létrehozhatjuk hello erőforráscsoportot.

* Hozzon létre egy erőforráscsoportot

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Ellenőrizze, hogy hello erőforráscsoport megfelelően lett kiépítve. **ProvisioningState** kell lennie "sikeresen befejeződött."
hello sablon toogenerate hello tárfióknév hello erőforráscsoport nevét használja. hello tárfiók neve 3 – 24 karakter hosszúságúnak kell és számokat és kisbetűket tartalmazhatnak csak.

![Erőforráscsoport][2]

* Egy új Machine Learning munkaterületének használatával hello erőforrás csoport központi telepítése, központi telepítése.

```
# Create a Resource Group, TemplateFile is hello location of hello JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Ha hello telepítés befejeződött, egyszerű tooaccess tulajdonságok telepített hello munkaterület. Például az elsődleges kulcs jogkivonata hello végezheti el.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Egy másik módja tooretrieve jogkivonatok meglévő munkaterület toouse hello Invoke-AzureRmResourceAction parancs. Például listázhatja hello elsődleges és másodlagos jogkivonatok összes munkaterületet.

```  
# List hello primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Hello munkaterület üzembe helyezése után is automatizálhatja a hello segítségével számos Azure Machine Learning Studio feladat [Azure Machine Learning PowerShell-modul](http://aka.ms/amlps).

## <a name="next-steps"></a>Következő lépések
* További információ [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md). 
* Tekintse meg a következő hello [Azure gyors üzembe helyezés sablonok tárházba](https://github.com/Azure/azure-quickstart-templates). 
* Ezt a videót kapcsolatos [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 

<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
