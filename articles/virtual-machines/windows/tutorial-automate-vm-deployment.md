---
title: a Windows Azure-ban aaaCustomize |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello egyéni parancsprogramok futtatására szolgáló bővítmény és a Windows-alapú virtuális gépek az Azure Key Vault toocustomize"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c03b2bb6d70875134c63ea2fe4c2e2c1777c2188
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-windows-virtual-machine-in-azure"></a>Hogyan toocustomize Windows virtuális gépként az Azure-ban
tooconfigure virtuális gépek (VM) gyors és konzisztens módon, valamilyen automatizált művelettel általában van szükség. Egy általános módszer toocustomize egy Windows virtuális gép toouse [egyéni parancsfájl kiterjesztése a Windows](extensions-customscript.md). Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Hello egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall IIS használata
> * Egyéni parancsprogramok futtatására szolgáló bővítmény hello használó virtuális gép létrehozása
> * Egy futó IIS-webhely megtekintése hello bővítmény alkalmazása után

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).


## <a name="custom-script-extension-overview"></a>Egyéni parancsfájl-bővítmény áttekintése
Egyéni parancsprogramok futtatására szolgáló bővítmény hello és hajtanak végre a parancsfájlok Azure virtuális gépeken. A bővítmény akkor hasznos, ha a feladás egy vagy több központi telepítés konfigurálása, a szoftver telepítése vagy a más beállításokat / kezelési feladatot. Parancsfájlok le: az Azure storage vagy a Githubon, vagy megadott toohello az Azure portálon, a bővítmény futási időt.

Egyéni parancsprogramok futtatására szolgáló bővítmény hello integrálódik az Azure Resource Manager-sablonok, és is futtathat a hello Azure CLI, PowerShell, Azure-portálon vagy hello Azure virtuális gép REST API használatával.

Windows és Linux virtuális gépek hello egyéni parancsprogramok futtatására szolgáló bővítmény használható.


## <a name="create-virtual-machine"></a>Virtuális gép létrehozása
A virtuális gépek létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAutomate* a hello *EastUS* helye:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

Állítsa a rendszergazda felhasználónevét és jelszavát hello rendelkező virtuális gépek [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Most hozhat létre a virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). hello alábbi példa létrehoz hello szükséges virtuális hálózati összetevők hello az operációs rendszer konfigurációs, és létrehoz egy nevű virtuális gép *myVM*:

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

Hello erőforrások és a létrehozott virtuális gép toobe néhány percet vesz igénybe.


## <a name="automate-iis-install"></a>Az IIS-telepítés automatizálása
Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello egyéni parancsprogramok futtatására szolgáló bővítmény. bővítmény futtatása hello `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webkiszolgálót, majd a frissítések hello *Default.htm* lap tooshow hello hello virtuális gép állomásnevét:

```powershell
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a>Teszt webhely
Hello nyilvános IP-cím a terheléselosztó az beszerzése [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello alábbi példa beszerzi hello IP-címet *myPublicIP* korábban létrehozott:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

Majd tooa webböngészőben hello nyilvános IP-címet adhat meg. hello webhely jelenik meg, beleértve a virtuális gép hello hello állomásnevét adott hello terheléselosztó forgalom tooas hello a következő példa az elosztott:

![Futó IIS-webhely](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban automatikus hello IIS telepítése egy virtuális Gépre. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Hello egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall IIS használata
> * Egyéni parancsprogramok futtatására szolgáló bővítmény hello használó virtuális gép létrehozása
> * Egy futó IIS-webhely megtekintése hello bővítmény alkalmazása után

Hogyan előzetes toohello következő útmutató toolearn toocreate egyéni Virtuálisgép-lemezképek.

> [!div class="nextstepaction"]
> [Egyéni virtuálisgép-rendszerképek létrehozása](./tutorial-custom-images.md)
