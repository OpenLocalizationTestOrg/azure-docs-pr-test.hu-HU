---
title: "a Windows virtuális gépek Azure-ban sablonból aaaCreate |} Microsoft Docs"
description: "A Resource Manager-sablont használ, és PowerShell tooeasily hozzon létre egy új Windows virtuális Gépet."
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
ms.openlocfilehash: 630111482c7dc046091632e2ed458ac143325d59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Windows rendszerű virtuális gép létrehozása egy Resource Manager sablon

Ez a cikk bemutatja, hogyan toodeploy Azure Resource Manager sablon PowerShell használatával. az Ön által létrehozott hello sablon telepíti a Windows Server rendszert futtató új virtuális hálózatot egyetlen alhálózattal egyetlen virtuális gép.

Virtuálisgép-erőforrás hello részletes ismertetését lásd: [Azure Resource Manager-sablonokban a virtuális gépek](template-description.md). A sablon erőforrásainak hello kapcsolatos további információkért lásd: [Azure Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Öt perc toodo hello ebben a cikkben ismertetett visszaállítási lépésekkel kell vennie.

## <a name="install-azure-powershell"></a>Az Azure PowerShell telepítése

Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](../../powershell-install-configure.md) hello Azure PowerShell legújabb verziójának telepítése, az előfizetés kiválasztásával és tooyour fiók bejelentkezés kapcsolatos információkat.

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Minden erőforrás kell telepíteni egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).

1. Szerezzen be egy listát az összes elérhető helyről, ahol erőforrásokat lehet létrehozni.
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. Hello erőforráscsoport kiválasztott hello a helyen hozza létre. Ez a példa bemutatja nevű erőforráscsoport létrehozása hello **myResourceGroup** a hello **USA nyugati régiója** helye:

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-hello-files"></a>Hello-fájlok létrehozása

Ebben a lépésben létrehoz egy sablonfájlban hello erőforrások műveletein és paraméterfájl, amely megadja az paraméter értékek toohello sablont. Is létrehozhat egy engedélyezési fájlt, amely használt tooperform Azure Resource Manager műveletek.

1. Hozzon létre egy fájlt *CreateVMTemplate.json* , és adja hozzá a JSON-kód tooit:

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

2. Hozzon létre egy fájlt *Parameters.json* , és adja hozzá a JSON-kód tooit:

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

3. Hozzon létre egy új tárfiók és tároló:

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. Töltse fel a hello fájlok toohello storage-fiók:

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    Változás hello - fájl elérési utak toohello helye hello fájlok tárolásához.

## <a name="create-hello-resources"></a>Hello erőforrások létrehozása

Hello paraméterek használatával hello sablon üzembe helyezése:

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> Sablonok és a paraméterek a helyi fájlokból is telepítheti. több, lásd: toolearn [Azure PowerShell használata az Azure Storage](../../storage/common/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Következő lépések

- Ha problémák merültek fel hello telepítési, előfordulhat, hogy vessen egy pillantást [hibaelhárítás általános az Azure-telepítés az Azure Resource Manager](../../resource-manager-common-deployment-errors.md).
- Megtudhatja, hogyan toocreate és kezelheti a virtuális gép [létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

