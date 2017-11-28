---
title: "Windows virtuális gép létrehozása az Azure-sablon alapján |} Microsoft Docs"
description: "A Resource Manager-sablon és a PowerShell segítségével egyszerűen hozzon létre egy új Windows virtuális Gépet."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: davidmu
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ddab80262fe27c1f5995858ec7de75d7c46df081
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a><span data-ttu-id="0c3c4-103">Windows rendszerű virtuális gép létrehozása egy Resource Manager sablon</span><span class="sxs-lookup"><span data-stu-id="0c3c4-103">Create a Windows virtual machine from a Resource Manager template</span></span>

<span data-ttu-id="0c3c4-104">Ez a cikk bemutatja, hogyan telepítheti az Azure Resource Manager-sablon PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-104">This article shows you how to deploy an Azure Resource Manager template using PowerShell.</span></span> <span data-ttu-id="0c3c4-105">A sablon az Ön által létrehozott telepít egy új virtuális hálózatot egyetlen alhálózattal Windows Server rendszerű egyetlen virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-105">The template that you create deploys a single virtual machine running Windows Server in a new virtual network with a single subnet.</span></span>

<span data-ttu-id="0c3c4-106">A virtuálisgép-erőforrás részletes ismertetését lásd: [Azure Resource Manager-sablonokban a virtuális gépek](template-description.md).</span><span class="sxs-lookup"><span data-stu-id="0c3c4-106">For a detailed description of the virtual machine resource, see [Virtual machines in an Azure Resource Manager template](template-description.md).</span></span> <span data-ttu-id="0c3c4-107">A sablon összes erőforrásokra vonatkozó további információkért lásd: [Azure Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="0c3c4-107">For more information about all the resources in a template, see [Azure Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="0c3c4-108">Öt perc kell vennie a cikkben leírt lépéseket tennie.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-108">It should take about five minutes to do the steps in this article.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="0c3c4-109">Az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="0c3c4-109">Install Azure PowerShell</span></span>

<span data-ttu-id="0c3c4-110">Az Azure PowerShell legfrissebb verziójának telepítésével, a kívánt előfizetés kiválasztásával és a fiókjába való bejelentkezéssel kapcsolatos információkért lásd: [How to install and configure Azure PowerShell](../../powershell-install-configure.md) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="0c3c4-110">See [How to install and configure Azure PowerShell](../../powershell-install-configure.md) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to your account.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="0c3c4-111">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="0c3c4-111">Create a resource group</span></span>

<span data-ttu-id="0c3c4-112">Minden erőforrás kell telepíteni egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0c3c4-112">All resources must be deployed in a [resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="0c3c4-113">Szerezzen be egy listát az összes elérhető helyről, ahol erőforrásokat lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-113">Get a list of available locations where resources can be created.</span></span>
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. <span data-ttu-id="0c3c4-114">Az erőforráscsoport létrehozása a kiválasztott helyen.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-114">Create the resource group in the location that you select.</span></span> <span data-ttu-id="0c3c4-115">Ez a példa bemutatja nevű erőforráscsoport **myResourceGroup** a a **USA nyugati régiója** helye:</span><span class="sxs-lookup"><span data-stu-id="0c3c4-115">This example shows the creation of a resource group named **myResourceGroup** in the **West US** location:</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-the-files"></a><span data-ttu-id="0c3c4-116">A fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0c3c4-116">Create the files</span></span>

<span data-ttu-id="0c3c4-117">Ebben a lépésben létrehoz egy sablonfájlban, amely telepíti az erőforrások és paraméterfájl, amely megadja az paraméterértékeket a sablonhoz.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-117">In this step, you create a template file that deploys the resources and a parameters file that supplies parameter values to the template.</span></span> <span data-ttu-id="0c3c4-118">Is létrehozhat egy engedélyezési fájlt, amely az Azure Resource Manager műveletek végrehajtásához használatos.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-118">You also create an authorization file that is used to perform Azure Resource Manager operations.</span></span>

1. <span data-ttu-id="0c3c4-119">Hozzon létre egy fájlt *CreateVMTemplate.json* , és ez a JSON-kód felvétele:</span><span class="sxs-lookup"><span data-stu-id="0c3c4-119">Create a file named *CreateVMTemplate.json* and add this JSON code to it:</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

2. <span data-ttu-id="0c3c4-120">Hozzon létre egy fájlt *Parameters.json* , és ez a JSON-kód felvétele:</span><span class="sxs-lookup"><span data-stu-id="0c3c4-120">Create a file named *Parameters.json* and add this JSON code to it:</span></span>

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

3. <span data-ttu-id="0c3c4-121">Hozzon létre egy új tárfiók és tároló:</span><span class="sxs-lookup"><span data-stu-id="0c3c4-121">Create a new storage account and container:</span></span>

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. <span data-ttu-id="0c3c4-122">A fájlok feltöltése a tárfiókba:</span><span class="sxs-lookup"><span data-stu-id="0c3c4-122">Upload the files to the storage account:</span></span>

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    <span data-ttu-id="0c3c4-123">Változás - fájl elérési útját a fájlok tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-123">Change the -File paths to the location where you stored the files.</span></span>

## <a name="create-the-resources"></a><span data-ttu-id="0c3c4-124">Az erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="0c3c4-124">Create the resources</span></span>

<span data-ttu-id="0c3c4-125">A sablon a paraméterek használatával telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="0c3c4-125">Deploy the template using the parameters:</span></span>

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> <span data-ttu-id="0c3c4-126">Sablonok és a paraméterek a helyi fájlokból is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="0c3c4-126">You can also deploy templates and parameters from local files.</span></span> <span data-ttu-id="0c3c4-127">További tudnivalókért lásd: [Azure PowerShell használata az Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="0c3c4-127">To learn more, see [Using Azure PowerShell with Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c3c4-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c3c4-128">Next Steps</span></span>

- <span data-ttu-id="0c3c4-129">Ha problémák merültek fel a központi telepítést, előfordulhat, hogy vessen egy pillantást [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="0c3c4-129">If there were issues with the deployment, you might take a look at [Troubleshoot common Azure deployment errors with Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span></span>
- <span data-ttu-id="0c3c4-130">Megtudhatja, hogyan hozhatja létre és kezelheti a virtuális gép [létrehozása és kezelése Windows virtuális gépek az Azure PowerShell modulra](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0c3c4-130">Learn how to create and manage a virtual machine in [Create and manage Windows VMs with the Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

