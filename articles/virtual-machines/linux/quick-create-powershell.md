---
title: "aaaAzure gyors üzembe helyezés - VM PowerShell létrehozása |} Microsoft Docs"
description: "Gyorsan további toocreate egy Linux virtuális gépek a PowerShell használatával"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f05ea7fedafe4fda660dc6084ae57ebf9dced473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-powershell"></a>Linux rendszerű virtuális gép létrehozása PowerShell segítségével

hello Azure PowerShell modul használt toocreate és hello PowerShell parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. Ez az útmutató adatokat hello Azure PowerShell modul toodeploy Ubuntu server operációs rendszert futtató virtuális gépek használata. Miután hello kiszolgáló van telepítve, az SSH-kapcsolat jön létre, és egy NGINX-webkiszolgálón telepítve van.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

A gyors üzembe helyezési hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

Végül, egy nyilvános SSH-kulcs hello nevű *id_rsa.pub* hello tárolt toobe kell *.ssh* könyvtárát, a Windows felhasználói profilt. Az Azure-hoz tartozó SSH-kulcsok létrehozásával kapcsolatos részletes információkért olvassa el az [SSH-kulcsok az Azure-hoz történő létrehozását](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ismertető cikket.

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be Azure előfizetés hello tooyour `Login-AzureRmAccount` parancsot, és kövesse hello képernyőn megjelenő utasításokat.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Erőforráscsoport létrehozása

Hozzon létre egy Azure-erőforráscsoportot a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmaggal. Az erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a>Hálózati erőforrások létrehozása

Hozzon létre egy virtuális hálózatot, egy alhálózatot és egy nyilvános IP-címet. Ezeket az erőforrásokat használt tooprovide hálózati kapcsolat toohello virtuális gépet, és csatlakoztassa toohello internet.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

Hozzon létre egy hálózati biztonsági csoportot és egy hálózati biztonsági csoportszabályt. hello hálózati biztonsági csoport hello virtuális gépet a bejövő és kimenő szabályok használatával titkosítja. Ebben az esetben létrejön egy bejövő szabály a 22-es porthoz, amely lehetővé teszi a bejövő SSH-kapcsolatokat. Szeretnénk továbbá egy bejövő forgalomra vonatkozó szabály toocreate port 80, amely lehetővé teszi a bejövő webes forgalomnak.

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

Hozzon létre egy hálózati kártya [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) hello virtuális géphez. hello hálózati kártya hello virtuális gép tooa alhálózat, a hálózati biztonsági csoport és a nyilvános IP-cím csatlakozik.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

Hozzon létre egy virtuálisgép-konfigurációt. A konfiguráció hello virtuális gépek például a virtuálisgép-lemezkép mérete és hitelesítési konfiguráció telepítésekor használt hello beállításokat tartalmazza.

```powershell
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName myVM -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName Canonical -Offer UbuntuServer -Skus 14.04.2-LTS -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

Hozzon létre virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a>Csatlakoztassa a gépet toovirtual

Hello központi telepítés befejezése után hozzon létre egy SSH-kapcsolat hello virtuális gépet.

Használjon hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) tooreturn hello nyilvános IP-cím hello virtuális gép parancsot.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

A telepített SSH rendszerből használt hello következő parancsot a tooconnect toohello virtuális gépet. Ha a Windows rendszeren működik [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) használt toocreate hello kapcsolat lehet. 

```bash 
ssh <Public IP Address>
```

Amikor a rendszer kéri, hello bejelentkezési felhasználói név az *azureuser*. Ha egy hozzáférési kódot adott meg, ha SSH-kulcsok létrehozása, tooenter ebben is kell.


## <a name="install-nginx"></a>Az NGINX telepítése

Használjon hello alábbi parancsfájl tooupdate csomag források bash, és hello legújabb NGINX csomag telepítése. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-ngix-welcome-page"></a>Hello NGIX üdvözlőlap megtekintése

Telepített NGINX és a virtuális gépre, az Internet - hello most nyissa meg a 80-as port az a választott tooview hello alapértelmezett NGINX üdvözlőlap webböngésző is használhatja. Lehet, hogy toouse hello nyilvános IP-cím feletti toovisit hello alapértelmezett oldal részletes ismertetését lásd. 

![Alapértelmezett NGINX-webhely](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szükség, használhatja a hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót. További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Linux virtuális gépek továbbra is.

> [!div class="nextstepaction"]
> [Azure-beli Linux rendszerű virtuális gépek – oktatóanyag](./tutorial-manage-vm.md)
