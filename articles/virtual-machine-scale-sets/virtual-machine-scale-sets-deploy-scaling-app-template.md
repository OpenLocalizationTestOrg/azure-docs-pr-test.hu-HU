---
title: "Alkalmazás üzembe helyezése Azure virtuálisgép-méretezési csoportban | Microsoft Docs"
description: "Elsajátíthatja, hogyan lehet üzembe helyezni egy automatikus méretezést végző egyszerű alkalmazást egy virtuálisgép-méretezési csoportban egy Azure Resource Manager-sablon használatával."
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
ms.openlocfilehash: 07883a33382cc660b043c99872312a9e77228253
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-autoscaling-app-using-a-template"></a><span data-ttu-id="961d0-103">Automatikus méretezést végző alkalmazás üzembe helyezése sablon használatával</span><span class="sxs-lookup"><span data-stu-id="961d0-103">Deploy an autoscaling app using a template</span></span>

<span data-ttu-id="961d0-104">Az [Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) remek megoldást kínálnak egymáshoz kapcsolódó erőforráscsoportok üzembe helyezésére.</span><span class="sxs-lookup"><span data-stu-id="961d0-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way to deploy groups of related resources.</span></span> <span data-ttu-id="961d0-105">Ez az oktatóanyag az [egyszerű méretezési csoport üzembe helyezését](virtual-machine-scale-sets-mvss-start.md) ismertető cikkre épít, és bemutatja, hogyan lehet üzembe helyezni egy automatikus méretezést végző egyszerű alkalmazást egy méretezési csoportban Azure Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="961d0-105">This tutorial builds on [Deploy a simple scale set](virtual-machine-scale-sets-mvss-start.md) and describes how to deploy a simple autoscaling application on a scale set using an Azure Resource Manager template.</span></span>  <span data-ttu-id="961d0-106">Az automatikus méretezést a PowerShell, a CLI vagy a portál használatával is beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="961d0-106">You can also set up autoscaling using PowerShell, CLI, or the portal.</span></span> <span data-ttu-id="961d0-107">További információért lásd az [automatikus méretezést áttekintő](virtual-machine-scale-sets-autoscale-overview.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="961d0-107">For more information, see [Autoscale overview](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

## <a name="two-quickstart-templates"></a><span data-ttu-id="961d0-108">Két gyorsindítási sablon</span><span class="sxs-lookup"><span data-stu-id="961d0-108">Two quickstart templates</span></span>
<span data-ttu-id="961d0-109">Amikor üzembe helyez egy méretezési csoportot, egy [virtuálisgép-bővítmény](../virtual-machines/virtual-machines-windows-extensions-features.md) segítségével új szoftvereket telepíthet a platformlemezképre.</span><span class="sxs-lookup"><span data-stu-id="961d0-109">When you deploy a scale set you can install new software on a platform image using a [VM Extension](../virtual-machines/virtual-machines-windows-extensions-features.md).</span></span> <span data-ttu-id="961d0-110">A virtuálisgép-bővítmény egy kisméretű alkalmazás, amely üzembe helyezés utáni konfigurációs és automatizálási feladatokat biztosít az Azure-beli virtuális gépeken, például lehetővé teszi egy alkalmazás üzembe helyezését.</span><span class="sxs-lookup"><span data-stu-id="961d0-110">A VM extension is a small application that provides post-deployment configuration and automation tasks on Azure virtual machines, such as deploying an app.</span></span> <span data-ttu-id="961d0-111">Az [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) sablonok két különböző mintasablont kínálnak, amelyek bemutatják, hogyan lehet üzembe helyezni egy automatikus méretezést végző alkalmazást egy méretezési csoportban virtuálisgép-bővítmények használatával.</span><span class="sxs-lookup"><span data-stu-id="961d0-111">Two different sample templates are provided in [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) which show how to deploy an autoscaling application onto a scale set using VM extensions.</span></span>

### <a name="python-http-server-on-linux"></a><span data-ttu-id="961d0-112">Python HTTP-kiszolgáló Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="961d0-112">Python HTTP server on Linux</span></span>
<span data-ttu-id="961d0-113">A [Python HTTP-kiszolgáló Linux rendszeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) mintasablon egy automatikus méretezést végző egyszerű alkalmazást helyez üzembe, amely egy Linux rendszerű méretezési csoporton fut.</span><span class="sxs-lookup"><span data-stu-id="961d0-113">The [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sample template deploys a simple autoscaling application running on a Linux scale set.</span></span>  <span data-ttu-id="961d0-114">A sablon egy egyéni virtuálisgép-szkriptbővítmény segítségével a méretezési csoport minden virtuális gépén üzembe helyezi a [Bottle](http://bottlepy.org/docs/dev/) Python webes keretrendszert és egy egyszerű HTTP-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="961d0-114">[Bottle](http://bottlepy.org/docs/dev/), a Python web framework, and a simple HTTP server are deployed on each VM in the scale set using a custom script VM extension.</span></span> <span data-ttu-id="961d0-115">A méretezési csoport akkor skálázódik fel, ha az átlagos processzorhasználat az összes virtuális gépen meghaladja a 60%-ot, és akkor skálázódik le, ha az átlagos processzorhasználat 30% alá esik.</span><span class="sxs-lookup"><span data-stu-id="961d0-115">The scale set scales up when average CPU utilization across all VMs is greater than 60% and scales down when the average CPU utilization is less than 30%.</span></span>

<span data-ttu-id="961d0-116">A méretezésicsoport-erőforrás mellett az *azuredeploy.json* mintasablon a virtuális hálózat, nyilvános IP-cím, terheléselosztó és automatikus méretezési beállítások erőforrásokat is deklarálja.</span><span class="sxs-lookup"><span data-stu-id="961d0-116">In addition to the scale set resource, the *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span>  <span data-ttu-id="961d0-117">Az erőforrások sablonban való létrehozásával kapcsolatos további információkért lásd [a Linux rendszerű méretezési csoporttal és az automatikus méretezéssel](virtual-machine-scale-sets-linux-autoscale.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="961d0-117">For more information on creating these resources in a template, see [Linux scale set with autoscale](virtual-machine-scale-sets-linux-autoscale.md).</span></span>

<span data-ttu-id="961d0-118">Az *azuredeploy.json* sablonban a `extensionProfile` erőforrás `Microsoft.Compute/virtualMachineScaleSets` tulajdonsága egy egyéni szkriptbővítményt határoz meg.</span><span class="sxs-lookup"><span data-stu-id="961d0-118">In the *azuredeploy.json* template, the `extensionProfile` property of the `Microsoft.Compute/virtualMachineScaleSets` resource specifies a custom script extension.</span></span> <span data-ttu-id="961d0-119">A `fileUris` a szkript(ek) helyét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="961d0-119">`fileUris` specifies the script(s) location.</span></span> <span data-ttu-id="961d0-120">Ebben az esetben két fájlról van szó, az egyik a *workserver.py*, amely egy egyszerű HTTP-kiszolgálót határoz meg, és az *installserver.sh*, amely telepíti a Bottle keretrendszert és elindítja a HTTP-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="961d0-120">In this case, two files: *workserver.py*, which defines a simple HTTP server, and *installserver.sh*, which installs Bottle and starts the HTTP server.</span></span> <span data-ttu-id="961d0-121">A `commandToExecute` a méretezési csoport üzembe helyezése után futtatandó parancsot határozza meg.</span><span class="sxs-lookup"><span data-stu-id="961d0-121">`commandToExecute` specifies the command to run after the scale set has been deployed.</span></span>

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

### <a name="aspnet-mvc-application-on-windows"></a><span data-ttu-id="961d0-122">ASP.NET MVC alkalmazás a Windows rendszeren</span><span class="sxs-lookup"><span data-stu-id="961d0-122">ASP.NET MVC application on Windows</span></span>
<span data-ttu-id="961d0-123">Az [ASP.NET MVC alkalmazás a Windows rendszeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) mintasablon egy egyszerű ASP.NET MVC alkalmazást helyez üzembe, amely az IIS-ben, egy Windows rendszerű méretezési csoporton fut.</span><span class="sxs-lookup"><span data-stu-id="961d0-123">The [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) sample template deploys a simple ASP.NET MVC app running in IIS on Windows scale set.</span></span>  <span data-ttu-id="961d0-124">Az IIS-t és az MVC alkalmazást a sablon a [PowerShell Desired State Configuration (DSC)](virtual-machine-scale-sets-dsc.md) virtuálisgép-bővítmény használatával helyezi üzembe.</span><span class="sxs-lookup"><span data-stu-id="961d0-124">IIS and the MVC app are deployed using the [PowerShell desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) VM extension.</span></span>  <span data-ttu-id="961d0-125">A méretezési csoport akkor skálázódik fel (egyszerre egy virtuálisgép-példányonként), ha a processzorhasználat 5 percen át 50% fölött van.</span><span class="sxs-lookup"><span data-stu-id="961d0-125">The scale set scales up (on VM instance at a time) when CPU utilization is greater than 50% for 5 minutes.</span></span> 

<span data-ttu-id="961d0-126">A méretezésicsoport-erőforrás mellett az *azuredeploy.json* mintasablon a virtuális hálózat, nyilvános IP-cím, terheléselosztó és automatikus méretezési beállítások erőforrásokat is deklarálja.</span><span class="sxs-lookup"><span data-stu-id="961d0-126">In addition to the scale set resource, the *azuredeploy.json* sample template also declares virtual network, public IP address, load balancer, and autoscale settings resources.</span></span> <span data-ttu-id="961d0-127">Ez a sablon az alkalmazásfrissítés módját is bemutatja.</span><span class="sxs-lookup"><span data-stu-id="961d0-127">This template also demonstrates application upgrade.</span></span>  <span data-ttu-id="961d0-128">Az erőforrások sablonban való létrehozásával kapcsolatos további információkért lásd [a Windows rendszerű méretezési csoporttal és az automatikus méretezéssel](virtual-machine-scale-sets-windows-autoscale.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="961d0-128">For more information on creating these resources in a template, see [Windows scale set with autoscale](virtual-machine-scale-sets-windows-autoscale.md).</span></span>

<span data-ttu-id="961d0-129">Az *azuredeploy.json* sablonban a `extensionProfile` erőforrás `Microsoft.Compute/virtualMachineScaleSets` tulajdonsága egy [Desired State Configuration (DSC)](virtual-machine-scale-sets-dsc.md) bővítményt határoz meg, amely telepíti az IIS-t és egy WebDeploy csomagból származó alapértelmezett webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="961d0-129">In the *azuredeploy.json* template, the `extensionProfile` property of the `Microsoft.Compute/virtualMachineScaleSets` resource specifies a [desired state configuration (DSC)](virtual-machine-scale-sets-dsc.md) extension which installs IIS and a default web app from a WebDeploy package.</span></span>  <span data-ttu-id="961d0-130">A *DSC* mappában található *IISInstall.ps1* szkript telepíti az IIS-t a virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="961d0-130">The *IISInstall.ps1* script installs IIS on the virtual machine and is found in the *DSC* folder.</span></span>  <span data-ttu-id="961d0-131">Az MVC webalkalmazás a *WebDeploy* mappában található.</span><span class="sxs-lookup"><span data-stu-id="961d0-131">The MVC web app is found in the *WebDeploy* folder.</span></span>  <span data-ttu-id="961d0-132">A telepítési szkript és a webalkalmazás elérési útja az *azuredeploy.parameters.json* fájl `powershelldscZip` és `webDeployPackage` paraméterében van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="961d0-132">The paths to the install script and the web app are defined in the `powershelldscZip` and `webDeployPackage` parameters in the *azuredeploy.parameters.json* file.</span></span> 

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

## <a name="deploy-the-template"></a><span data-ttu-id="961d0-133">A sablon üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="961d0-133">Deploy the template</span></span>
<span data-ttu-id="961d0-134">A [Python HTTP-kiszolgáló Linux rendszeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) vagy az [ASP.NET MVC alkalmazás a Windows rendszeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) sablon üzembe helyezésének legegyszerűbb módja a GitHub információs fájljaiban található **Deploy to Azure** (Üzembe helyezés az Azure-ban) gomb használata.</span><span class="sxs-lookup"><span data-stu-id="961d0-134">The simplest way to deploy the [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) template is to use the **Deploy to Azure** button found in the in the readme files in GitHub.</span></span>  <span data-ttu-id="961d0-135">A mintasablonokat a PowerShell vagy az Azure CLI segítségével is üzembe helyezheti.</span><span class="sxs-lookup"><span data-stu-id="961d0-135">You can also use PowerShell or Azure CLI to deploy the sample templates.</span></span>

### <a name="powershell"></a><span data-ttu-id="961d0-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="961d0-136">PowerShell</span></span>
<span data-ttu-id="961d0-137">Másolja át a [Python HTTP-kiszolgáló Linux rendszeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) vagy az [ASP.NET MVC alkalmazás a Windows rendszeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) fájljait a GitHub-adattárból a helyi számítógép egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="961d0-137">Copy the [Python HTTP server on Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) or [ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) files from the GitHub repo to a folder on your local computer.</span></span>  <span data-ttu-id="961d0-138">Nyissa meg az *azuredeploy.parameters.json* fájlt, és frissítse a `vmssName`, `adminUsername` és `adminPassword` paraméterek alapértelmezett értékeit.</span><span class="sxs-lookup"><span data-stu-id="961d0-138">Open the *azuredeploy.parameters.json* file and update the default values of the `vmssName`, `adminUsername`, and `adminPassword` parameters.</span></span> <span data-ttu-id="961d0-139">Mentse a következő PowerShell-szkriptet a *deploy.ps1* fájlba, ugyanabba a mappába, amelyben az *azuredeploy.json* sablon található.</span><span class="sxs-lookup"><span data-stu-id="961d0-139">Save the following PowerShell script to *deploy.ps1* in the same folder as the *azuredeploy.json* template.</span></span> <span data-ttu-id="961d0-140">A mintasablon üzembe helyezéséhez futtassa a *deploy.ps1* szkriptet egy PowerShell-parancsablakból.</span><span class="sxs-lookup"><span data-stu-id="961d0-140">To deploy the sample template run the *deploy.ps1* script from a PowerShell command window.</span></span>

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
    Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start the deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

### <a name="azure-cli"></a><span data-ttu-id="961d0-141">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="961d0-141">Azure CLI</span></span>
```azurecli
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# -e: immediately exit if any command has a non-zero exit status
# -o: prevents errors in a pipeline from being masked
# IFS new value is less likely to cause confusing bugs when looping arrays or arguments (e.g. $@)

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
    echo "Enter a location below to create a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file to be used
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

#login to azure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set the default subscription id
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

## <a name="next-steps"></a><span data-ttu-id="961d0-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="961d0-142">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
