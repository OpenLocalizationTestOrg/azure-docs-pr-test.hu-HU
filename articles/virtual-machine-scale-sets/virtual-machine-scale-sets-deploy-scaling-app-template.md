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
# <a name="deploy-an-autoscaling-app-using-a-template"></a><span data-ttu-id="43ca9-103">Automatikus méretezést végző alkalmazás üzembe helyezése sablon használatával</span><span class="sxs-lookup"><span data-stu-id="43ca9-103">Deploy an autoscaling app using a template</span></span>

<span data-ttu-id="43ca9-104">[Az Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) vannak egy kiváló módja toodeploy kapcsolódó erőforrások csoportja.</span><span class="sxs-lookup"><span data-stu-id="43ca9-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way toodeploy groups of related resources.</span></span> <span data-ttu-id="43ca9-105">Ez az oktatóanyag épül [központi telepítése egy egyszerű méretezési](virtual-machine-scale-sets-mvss-start.md) , és bemutatja, hogyan toodeploy egy egyszerű automatikus skálázás alkalmazás szinten állíthatja Azure Resource Manager-sablonnal.</span><span class="sxs-lookup"><span data-stu-id="43ca9-105">This tutorial builds on [Deploy a simple scale set](virtual-machine-scale-sets-mvss-start.md) and describes how toodeploy a simple autoscaling application on a scale set using an Azure Resource Manager template.</span></span>  <span data-ttu-id="43ca9-106">Automatikus skálázás a PowerShell, a parancssori felületen vagy a hello portál is állíthat be.</span><span class="sxs-lookup"><span data-stu-id="43ca9-106">You can also set up autoscaling using PowerShell, CLI, or hello portal.</span></span> <span data-ttu-id="43ca9-107">További információért lásd az [automatikus méretezést áttekintő](virtual-machine-scale-sets-autoscale-overview.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="43ca9-107">For more information, see [Autoscale overview](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

## <a name="two-quickstart-templates"></a><span data-ttu-id="43ca9-108">Két gyorsindítási sablon</span><span class="sxs-lookup"><span data-stu-id="43ca9-108">Two quickstart templates</span></span>
<span data-ttu-id="43ca9-109">Amikor üzembe helyez egy méretezési csoportot, egy [virtuálisgép-bővítmény](../virtual-machines/virtual-machines-windows-extensions-features.md) segítségével új szoftvereket telepíthet a platformlemezképre.</span><span class="sxs-lookup"><span data-stu-id="43ca9-109">When you deploy a scale set you can install new software on a platform image using a [VM Extension](../virtual-machines/virtual-machines-windows-extensions-features.md).</span></span> <span data-ttu-id="43ca9-110">A virtuálisgép-bővítmény egy kisméretű alkalmazás, amely üzembe helyezés utáni konfigurációs és automatizálási feladatokat biztosít az Azure-beli virtuális gépeken, például lehetővé teszi egy alkalmazás üzembe helyezését.</span><span class="sxs-lookup"><span data-stu-id="43ca9-110">A VM extension is a small application that provides post-deployment configuration and automation tasks on Azure virtual machines, such as deploying an app.</span></span> <span data-ttu-id="43ca9-111">Két különböző minta sablonok találhatók: [Azure/azure-gyors üzembe helyezés-sablonok](https://github.com/Azure/azure-quickstart-templates) amely megjelenítése toodeploy alakzatot egy méretezési kérelmet automatikus skálázás beállításának módját Virtuálisgép-bővítmények használatával.</span><span class="sxs-lookup"><span data-stu-id="43ca9-111">Two different sample templates are provided in [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) which show how toodeploy an autoscaling application onto a scale set using VM extensions.</span></span>

### <a name="python-http-server-on-linux"></a><span data-ttu-id="43ca9-112">Python HTTP-kiszolgáló Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="43ca9-112">Python HTTP server on Linux</span></span>
<span data-ttu-id="43ca9-113">Hello [Python HTTP-kiszolgáló Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) mintasablon egy Linux-méretezési csoport egy egyszerű automatikus skálázás alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="43ca9-113">hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sample template deploys a simple autoscaling application running on a Linux scale set.</span></span>  <span data-ttu-id="43ca9-114">[Bottle](http://bottlepy.org/docs/dev/), a Python webes keretrendszer, és egy egyszerű HTTP-kiszolgáló hello méretezési készletben, Virtuálisgép-bővítmény egyéni parancsfájl használata a virtuális gépek vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="43ca9-114">[Bottle](http://bottlepy.org/docs/dev/), a Python web framework, and a simple HTTP server are deployed on each VM in hello scale set using a custom script VM extension.</span></span> <span data-ttu-id="43ca9-115">hello méretezési beállítani méretezik minden virtuális gép közötti átlagos CPU-felhasználás nagyobb, mint 60 % és arányosan csökken, ha hello átlagos processzorkihasználtság 30 %-nál kisebb.</span><span class="sxs-lookup"><span data-stu-id="43ca9-115">hello scale set scales up when average CPU utilization across all VMs is greater than 60% and scales down when hello average CPU utilization is less than 30%.</span></span>

<span data-ttu-id="43ca9-116">Ezenkívül toohello méretezési erőforrás, hello *azuredeploy.json* mintasablon is deklarálja a virtuális hálózati, a nyilvános IP-cím, a terheléselosztó és az automatikus skálázási beállításokat erőforrások.</span><span class="sxs-lookup"><span data-stu-id="43ca9-116">In addition toohello scale set resource, hello *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span>  <span data-ttu-id="43ca9-117">Az erőforrások sablonban való létrehozásával kapcsolatos további információkért lásd [a Linux rendszerű méretezési csoporttal és az automatikus méretezéssel](virtual-machine-scale-sets-linux-autoscale.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="43ca9-117">For more information on creating these resources in a template, see [Linux scale set with autoscale](virtual-machine-scale-sets-linux-autoscale.md).</span></span>

<span data-ttu-id="43ca9-118">A hello *azuredeploy.json* sablon, hello `extensionProfile` hello tulajdonságának `Microsoft.Compute/virtualMachineScaleSets` erőforrás egy egyéni parancsprogramok futtatására szolgáló bővítmény határozza meg.</span><span class="sxs-lookup"><span data-stu-id="43ca9-118">In hello *azuredeploy.json* template, hello `extensionProfile` property of hello `Microsoft.Compute/virtualMachineScaleSets` resource specifies a custom script extension.</span></span> <span data-ttu-id="43ca9-119">`fileUris`hello parancsfájl(ok) helyet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="43ca9-119">`fileUris` specifies hello script(s) location.</span></span> <span data-ttu-id="43ca9-120">Ebben az esetben két fájl: *workserver.py*, amely megadja, hogy egy egyszerű HTTP-kiszolgáló és *installserver.sh*, amely telepíti a Bottle, és elindítja hello HTTP-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="43ca9-120">In this case, two files: *workserver.py*, which defines a simple HTTP server, and *installserver.sh*, which installs Bottle and starts hello HTTP server.</span></span> <span data-ttu-id="43ca9-121">`commandToExecute`Adja meg a hello parancs toorun hello méretezési csoport telepítése után.</span><span class="sxs-lookup"><span data-stu-id="43ca9-121">`commandToExecute` specifies hello command toorun after hello scale set has been deployed.</span></span>

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

### <a name="aspnet-mvc-application-on-windows"></a><span data-ttu-id="43ca9-122">ASP.NET MVC alkalmazás a Windows rendszeren</span><span class="sxs-lookup"><span data-stu-id="43ca9-122">ASP.NET MVC application on Windows</span></span>
<span data-ttu-id="43ca9-123">Hello [ASP.NET MVC alkalmazás Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) mintasablon telepíti egy egyszerű ASP.NET MVC alkalmazást az IIS-ben futó Windows-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="43ca9-123">hello [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) sample template deploys a simple ASP.NET MVC app running in IIS on Windows scale set.</span></span>  <span data-ttu-id="43ca9-124">IIS hello MVC alkalmazás telepítve van a hello segítségével [PowerShell célállapot-konfiguráló (DSC)](virtual-machine-scale-sets-dsc.md) Virtuálisgép-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="43ca9-124">IIS and hello MVC app are deployed using hello [PowerShell desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) VM extension.</span></span>  <span data-ttu-id="43ca9-125">hello méretezési beállítása méretezik (a Virtuálisgép-példány egyszerre), ha a processzorkihasználtság 50 %-nál nagyobb 5 percig.</span><span class="sxs-lookup"><span data-stu-id="43ca9-125">hello scale set scales up (on VM instance at a time) when CPU utilization is greater than 50% for 5 minutes.</span></span> 

<span data-ttu-id="43ca9-126">Ezenkívül toohello méretezési erőforrás, hello *azuredeploy.json* mintasablon is deklarálja a virtuális hálózati, a nyilvános IP-cím, a terheléselosztó és az automatikus skálázási beállításokat erőforrások.</span><span class="sxs-lookup"><span data-stu-id="43ca9-126">In addition toohello scale set resource, hello *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span> <span data-ttu-id="43ca9-127">Ez a sablon az alkalmazásfrissítés módját is bemutatja.</span><span class="sxs-lookup"><span data-stu-id="43ca9-127">This template also demonstrates application upgrade.</span></span>  <span data-ttu-id="43ca9-128">Az erőforrások sablonban való létrehozásával kapcsolatos további információkért lásd [a Windows rendszerű méretezési csoporttal és az automatikus méretezéssel](virtual-machine-scale-sets-windows-autoscale.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="43ca9-128">For more information on creating these resources in a template, see [Windows scale set with autoscale](virtual-machine-scale-sets-windows-autoscale.md).</span></span>

<span data-ttu-id="43ca9-129">A hello *azuredeploy.json* sablon, hello `extensionProfile` hello tulajdonságának `Microsoft.Compute/virtualMachineScaleSets` erőforrás megadja egy [kívánt állapot konfigurációs (szolgáltatása DSC)](virtual-machine-scale-sets-dsc.md) -kiterjesztésen, amely telepíti az IIS és az alapértelmezett a webalkalmazás a WebDeploy-csomagból.</span><span class="sxs-lookup"><span data-stu-id="43ca9-129">In hello *azuredeploy.json* template, hello `extensionProfile` property of hello `Microsoft.Compute/virtualMachineScaleSets` resource specifies a [desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) extension which installs IIS and a default web app from a WebDeploy package.</span></span>  <span data-ttu-id="43ca9-130">Hello *IISInstall.ps1* parancsfájl hello virtuális gépre telepíti az IIS szolgáltatást, és hello található *DSC* mappát.</span><span class="sxs-lookup"><span data-stu-id="43ca9-130">hello *IISInstall.ps1* script installs IIS on hello virtual machine and is found in hello *DSC* folder.</span></span>  <span data-ttu-id="43ca9-131">hello MVC webalkalmazás megtalálható hello *WebDeploy* mappa.</span><span class="sxs-lookup"><span data-stu-id="43ca9-131">hello MVC web app is found in hello *WebDeploy* folder.</span></span>  <span data-ttu-id="43ca9-132">hello elérési utak toohello telepítési parancsfájlt és hello webalkalmazás definiált hello `powershelldscZip` és `webDeployPackage` hello paramétereiben *azuredeploy.parameters.json* fájlt.</span><span class="sxs-lookup"><span data-stu-id="43ca9-132">hello paths toohello install script and hello web app are defined in hello `powershelldscZip` and `webDeployPackage` parameters in hello *azuredeploy.parameters.json* file.</span></span> 

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

## <a name="deploy-hello-template"></a><span data-ttu-id="43ca9-133">Hello sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="43ca9-133">Deploy hello template</span></span>
<span data-ttu-id="43ca9-134">hello legegyszerűbb módja toodeploy hello [Python HTTP-kiszolgáló Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) vagy [ASP.NET MVC alkalmazás Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) sablon toouse hello **tooAzure telepítése** hello található gomb hello readme fájlokban a Githubon.</span><span class="sxs-lookup"><span data-stu-id="43ca9-134">hello simplest way toodeploy hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) template is toouse hello **Deploy tooAzure** button found in hello in hello readme files in GitHub.</span></span>  <span data-ttu-id="43ca9-135">PowerShell vagy Azure CLI toodeploy hello minta sablonokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="43ca9-135">You can also use PowerShell or Azure CLI toodeploy hello sample templates.</span></span>

### <a name="powershell"></a><span data-ttu-id="43ca9-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="43ca9-136">PowerShell</span></span>
<span data-ttu-id="43ca9-137">Másolás hello [Python HTTP-kiszolgáló Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) vagy [ASP.NET MVC alkalmazás Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) hello GitHub tárház tooa mappa fájljait a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="43ca9-137">Copy hello [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) files from hello GitHub repo tooa folder on your local computer.</span></span>  <span data-ttu-id="43ca9-138">Nyissa meg hello *azuredeploy.parameters.json* fájl- és frissítési hello alapértelmezett értékekkel rendelkező hello `vmssName`, `adminUsername`, és `adminPassword` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="43ca9-138">Open hello *azuredeploy.parameters.json* file and update hello default values of hello `vmssName`, `adminUsername`, and `adminPassword` parameters.</span></span> <span data-ttu-id="43ca9-139">Hello túl a következő PowerShell-parancsfájl mentése*deploy.ps1* hello a mappában, amelyben hello *azuredeploy.json* sablont.</span><span class="sxs-lookup"><span data-stu-id="43ca9-139">Save hello following PowerShell script too*deploy.ps1* in hello same folder as hello *azuredeploy.json* template.</span></span> <span data-ttu-id="43ca9-140">toodeploy hello minta futtatása sablon hello *deploy.ps1* parancsfájlt a PowerShell-parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="43ca9-140">toodeploy hello sample template run hello *deploy.ps1* script from a PowerShell command window.</span></span>

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

### <a name="azure-cli"></a><span data-ttu-id="43ca9-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="43ca9-141">Azure CLI</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="43ca9-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43ca9-142">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
