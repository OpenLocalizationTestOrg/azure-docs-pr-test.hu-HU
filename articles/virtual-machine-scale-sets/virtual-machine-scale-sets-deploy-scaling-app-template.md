---
title: "egy alkalmazásét az egy Azure virtuálisgép-méretezési csoport aaaDeploy |} Microsoft Docs"
description: "Ismerje meg, hogy egy egyszerű automatikus skálázás alkalmazás beállítása az Azure Resource Manager-sablonnal virtuálisgép-méretezési toodeploy."
services: virtual-machine-scale-sets
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: 6fccc310312cabfcdddfcbcd2d154fc5cc440417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-autoscaling-app-using-a-template"></a>Automatikus méretezést végző alkalmazás üzembe helyezése sablon használatával

[Az Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) vannak egy kiváló módja toodeploy kapcsolódó erőforrások csoportja. Ez az oktatóanyag épül [központi telepítése egy egyszerű méretezési](virtual-machine-scale-sets-mvss-start.md) , és bemutatja, hogyan toodeploy egy egyszerű automatikus skálázás alkalmazás szinten állíthatja Azure Resource Manager-sablonnal.  Automatikus skálázás a PowerShell, a parancssori felületen vagy a hello portál is állíthat be. További információért lásd az [automatikus méretezést áttekintő](virtual-machine-scale-sets-autoscale-overview.md) témakört.

## <a name="two-quickstart-templates"></a>Két gyorsindítási sablon
Amikor üzembe helyez egy méretezési csoportot, egy [virtuálisgép-bővítmény](../virtual-machines/virtual-machines-windows-extensions-features.md) segítségével új szoftvereket telepíthet a platformlemezképre. A virtuálisgép-bővítmény egy kisméretű alkalmazás, amely üzembe helyezés utáni konfigurációs és automatizálási feladatokat biztosít az Azure-beli virtuális gépeken, például lehetővé teszi egy alkalmazás üzembe helyezését. Két különböző minta sablonok találhatók: [Azure/azure-gyors üzembe helyezés-sablonok](https://github.com/Azure/azure-quickstart-templates) amely megjelenítése toodeploy alakzatot egy méretezési kérelmet automatikus skálázás beállításának módját Virtuálisgép-bővítmények használatával.

### <a name="python-http-server-on-linux"></a>Python HTTP-kiszolgáló Linux rendszeren
Hello [Python HTTP-kiszolgáló Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) mintasablon egy Linux-méretezési csoport egy egyszerű automatikus skálázás alkalmazást telepíti.  [Bottle](http://bottlepy.org/docs/dev/), a Python webes keretrendszer, és egy egyszerű HTTP-kiszolgáló hello méretezési készletben, Virtuálisgép-bővítmény egyéni parancsfájl használata a virtuális gépek vannak telepítve. hello méretezési beállítani méretezik minden virtuális gép közötti átlagos CPU-felhasználás nagyobb, mint 60 % és arányosan csökken, ha hello átlagos processzorkihasználtság 30 %-nál kisebb.

Ezenkívül toohello méretezési erőforrás, hello *azuredeploy.json* mintasablon is deklarálja a virtuális hálózati, a nyilvános IP-cím, a terheléselosztó és az automatikus skálázási beállításokat erőforrások.  Az erőforrások sablonban való létrehozásával kapcsolatos további információkért lásd [a Linux rendszerű méretezési csoporttal és az automatikus méretezéssel](virtual-machine-scale-sets-linux-autoscale.md) foglalkozó cikket.

A hello *azuredeploy.json* sablon, hello `extensionProfile` hello tulajdonságának `Microsoft.Compute/virtualMachineScaleSets` erőforrás egy egyéni parancsprogramok futtatására szolgáló bővítmény határozza meg. `fileUris`hello parancsfájl(ok) helyet határoz meg. Ebben az esetben két fájl: *workserver.py*, amely megadja, hogy egy egyszerű HTTP-kiszolgáló és *installserver.sh*, amely telepíti a Bottle, és elindítja hello HTTP-kiszolgáló. `commandToExecute`Adja meg a hello parancs toorun hello méretezési csoport telepítése után.

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "lapextension",
                "properties": {
                  "publisher": "Microsoft.Azure.Extensions",
                  "type": "CustomScript",
                  "typeHandlerVersion": "2.0",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "fileUris": [
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
                    ],
                    "commandToExecute": "bash installserver.sh"
                  }
                }
              }
            ]
          }
```

### <a name="aspnet-mvc-application-on-windows"></a>ASP.NET MVC alkalmazás a Windows rendszeren
Hello [ASP.NET MVC alkalmazás Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) mintasablon telepíti egy egyszerű ASP.NET MVC alkalmazást az IIS-ben futó Windows-méretezési készlet.  IIS hello MVC alkalmazás telepítve van a hello segítségével [PowerShell célállapot-konfiguráló (DSC)](virtual-machine-scale-sets-dsc.md) Virtuálisgép-bővítmény.  hello méretezési beállítása méretezik (a Virtuálisgép-példány egyszerre), ha a processzorkihasználtság 50 %-nál nagyobb 5 percig. 

Ezenkívül toohello méretezési erőforrás, hello *azuredeploy.json* mintasablon is deklarálja a virtuális hálózati, a nyilvános IP-cím, a terheléselosztó és az automatikus skálázási beállításokat erőforrások. Ez a sablon az alkalmazásfrissítés módját is bemutatja.  Az erőforrások sablonban való létrehozásával kapcsolatos további információkért lásd [a Windows rendszerű méretezési csoporttal és az automatikus méretezéssel](virtual-machine-scale-sets-windows-autoscale.md) foglalkozó cikket.

A hello *azuredeploy.json* sablon, hello `extensionProfile` hello tulajdonságának `Microsoft.Compute/virtualMachineScaleSets` erőforrás megadja egy [kívánt állapot konfigurációs (szolgáltatása DSC)](virtual-machine-scale-sets-dsc.md) -kiterjesztésen, amely telepíti az IIS és az alapértelmezett a webalkalmazás a WebDeploy-csomagból.  Hello *IISInstall.ps1* parancsfájl hello virtuális gépre telepíti az IIS szolgáltatást, és hello található *DSC* mappát.  hello MVC webalkalmazás megtalálható hello *WebDeploy* mappa.  hello elérési utak toohello telepítési parancsfájlt és hello webalkalmazás definiált hello `powershelldscZip` és `webDeployPackage` hello paramétereiben *azuredeploy.parameters.json* fájlt. 

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Powershell.DSC",
                "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.9",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('powershelldscUpdateTagVersion')]",
                  "settings": {
                    "configuration": {
                      "url": "[variables('powershelldscZipFullPath')]",
                      "script": "IISInstall.ps1",
                      "function": "InstallIIS"
                    },
                    "configurationArguments": {
                      "nodeName": "localhost",
                      "WebDeployPackagePath": "[variables('webDeployPackageFullPath')]"
                    }
                  }
                }
              }
            ]
          }
```

## <a name="deploy-hello-template"></a>Hello sablon üzembe helyezése
hello legegyszerűbb módja toodeploy hello [Python HTTP-kiszolgáló Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) vagy [ASP.NET MVC alkalmazás Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) sablon toouse hello **tooAzure telepítése** hello található gomb hello readme fájlokban a Githubon.  PowerShell vagy Azure CLI toodeploy hello minta sablonokat is használhat.

### <a name="powershell"></a>PowerShell
Másolás hello [Python HTTP-kiszolgáló Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) vagy [ASP.NET MVC alkalmazás Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) hello GitHub tárház tooa mappa fájljait a helyi számítógépen.  Nyissa meg hello *azuredeploy.parameters.json* fájl- és frissítési hello alapértelmezett értékekkel rendelkező hello `vmssName`, `adminUsername`, és `adminPassword` paraméterek. Hello túl a következő PowerShell-parancsfájl mentése*deploy.ps1* hello a mappában, amelyben hello *azuredeploy.json* sablont. toodeploy hello minta futtatása sablon hello *deploy.ps1* parancsfájlt a PowerShell-parancsablakot.

```powershell
param(
 [Parameter(Mandatory=$True)]
 [string]
 $subscriptionId,

 [Parameter(Mandatory=$True)]
 [string]
 $resourceGroupName,

 [string]
 $resourceGroupLocation,

 [Parameter(Mandatory=$True)]
 [string]
 $deploymentName,

 [string]
 $templateFilePath = "template.json",

 [string]
 $parametersFilePath = "parameters.json"
)

<#
.SYNOPSIS
    Registers RPs
#>
Function RegisterRP {
    Param(
        [string]$ResourceProviderNamespace
    )

    Write-Host "Registering resource provider '$ResourceProviderNamespace'";
    Register-AzureRmResourceProvider -ProviderNamespace $ResourceProviderNamespace;
}

#******************************************************************************
# Script body
# Execution begins here
#******************************************************************************
$ErrorActionPreference = "Stop"

# sign in
Write-Host "Logging in...";
Login-AzureRmAccount;

# select subscription
Write-Host "Selecting subscription '$subscriptionId'";
Select-AzureRmSubscription -SubscriptionID $subscriptionId;

# Register RPs
$resourceProviders = @("microsoft.compute","microsoft.insights","microsoft.network");
if($resourceProviders.length) {
    Write-Host "Registering resource providers"
    foreach($resourceProvider in $resourceProviders) {
        RegisterRP($resourceProvider);
    }
}

#Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
if(!$resourceGroup)
{
    Write-Host "Resource group '$resourceGroupName' does not exist. toocreate a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start hello deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

### <a name="azure-cli"></a>Azure CLI
```azurecli
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# -e: immediately exit if any command has a non-zero exit status
# -o: prevents errors in a pipeline from being masked
# IFS new value is less likely toocause confusing bugs when looping arrays or arguments (e.g. $@)

usage() { echo "Usage: $0 -i <subscriptionId> -g <resourceGroupName> -n <deploymentName> -l <resourceGroupLocation>" 1>&2; exit 1; }

declare subscriptionId=""
declare resourceGroupName=""
declare deploymentName=""
declare resourceGroupLocation=""

# Initialize parameters specified from command line
while getopts ":i:g:n:l:" arg; do
    case "${arg}" in
        i)
            subscriptionId=${OPTARG}
            ;;
        g)
            resourceGroupName=${OPTARG}
            ;;
        n)
            deploymentName=${OPTARG}
            ;;
        l)
            resourceGroupLocation=${OPTARG}
            ;;
        esac
done
shift $((OPTIND-1))

#Prompt for parameters is some required parameters are missing
if [[ -z "$subscriptionId" ]]; then
    echo "Subscription Id:"
    read subscriptionId
    [[ "${subscriptionId:?}" ]]
fi

if [[ -z "$resourceGroupName" ]]; then
    echo "ResourceGroupName:"
    read resourceGroupName
    [[ "${resourceGroupName:?}" ]]
fi

if [[ -z "$deploymentName" ]]; then
    echo "DeploymentName:"
    read deploymentName
fi

if [[ -z "$resourceGroupLocation" ]]; then
    echo "Enter a location below toocreate a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file toobe used
templateFilePath="template.json"

if [ ! -f "$templateFilePath" ]; then
    echo "$templateFilePath not found"
    exit 1
fi

#parameter file path
parametersFilePath="parameters.json"

if [ ! -f "$parametersFilePath" ]; then
    echo "$parametersFilePath not found"
    exit 1
fi

if [ -z "$subscriptionId" ] || [ -z "$resourceGroupName" ] || [ -z "$deploymentName" ]; then
    echo "Either one of subscriptionId, resourceGroupName, deploymentName is empty"
    usage
fi

#login tooazure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set hello default subscription id
az account set --name $subscriptionId

set +e

#Check for existing RG
az group show $resourceGroupName 1> /dev/null

if [ $? != 0 ]; then
    echo "Resource group with name" $resourceGroupName "could not be found. Creating new resource group.."
    set -e
    (
        set -x
        az group create --name $resourceGroupName --location $resourceGroupLocation 1> /dev/null
    )
    else
    echo "Using existing resource group..."
fi

#Start deployment
echo "Starting deployment..."
(
    set -x
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file $templateFilePath --parameters $parametersFilePath
)

if [ $?  == 0 ];
 then
    echo "Template has been successfully deployed"
fi
```

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
