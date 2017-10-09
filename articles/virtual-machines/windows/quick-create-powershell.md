---
title: "gyors üzembe helyezés – aaaAzure létrehozása Windows virtuális gép PowerShell |} Microsoft Docs"
description: "Gyorsan további toocreate egy Windows virtuális gépek a PowerShell használatával"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5e92435bf7ee443a01c158fed91c356363e2f425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell"></a>Windows rendszerű virtuális gép létrehozása PowerShell használatával

hello Azure PowerShell modul használt toocreate és hello PowerShell parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. Ez az útmutató adatokat PowerShell toocreate és a Windows Server 2016 futó Azure virtuális géphez. Központi telepítés befejezése után azt csatlakoztassa toohello a kiszolgálót, és telepítse az IIS.  

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

A gyors üzembe helyezési hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure

Jelentkezzen be Azure előfizetés hello tooyour `Login-AzureRmAccount` parancsot, és kövesse hello képernyőn megjelenő utasításokat.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Erőforráscsoport létrehozása

Hozzon létre egy Azure-erőforráscsoportot a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmaggal. Az erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-networking-resources"></a>Hálózati erőforrások létrehozása

### <a name="create-a-virtual-network-subnet-and-a-public-ip-address"></a>Hozzon létre egy virtuális hálózatot, egy alhálózatot és egy nyilvános IP-címet. 
Ezeket az erőforrásokat használt tooprovide hálózati kapcsolat toohello virtuális gépet, és csatlakoztassa toohello internet.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location EastUS `
    -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location EastUS `
    -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Hozzon létre egy hálózati biztonsági csoportot és egy hálózati biztonsági csoportszabályt. 
hello hálózati biztonsági csoport hello virtuális gépet a bejövő és kimenő szabályok használatával titkosítja. Ebben az esetben létrejön egy bejövő szabály a 3389-es porthoz, amely lehetővé teszi a bejövő távoli asztali kapcsolatokat. Szeretnénk továbbá egy bejövő forgalomra vonatkozó szabály toocreate port 80, amely lehetővé teszi a bejövő webes forgalomnak.

```powershell
# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
    -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location EastUS `
    -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP,$nsgRuleWeb
```

### <a name="create-a-network-card-for-hello-virtual-machine"></a>A hálózati kártya hello virtuális gép létrehozása. 
Hozzon létre egy hálózati kártya [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) hello virtuális géphez. hello hálózati kártya hello virtuális gép tooa alhálózat, a hálózati biztonsági csoport és a nyilvános IP-cím csatlakozik.

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

Hozzon létre egy virtuálisgép-konfigurációt. A konfiguráció hello virtuális gépek például a virtuálisgép-lemezkép mérete és hitelesítési konfiguráció telepítésekor használt hello beállításokat tartalmazza. Ennek a lépésnek a futtatásakor a rendszer a hitelesítő adatok megadását kéri. megadott hello értékek hello felhasználónevet és jelszót hello virtuális géphez van konfigurálva.

```powershell
# Define a credential object
$cred = Get-Credential

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Hozzon létre virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location EastUS -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a>Csatlakoztassa a gépet toovirtual

Hello telepítés befejeződése után létre kell hoznia egy távoli asztali kapcsolat hello virtuális géppel.

Használjon hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) tooreturn hello nyilvános IP-cím hello virtuális gép parancsot. Jegyezze fel ezt az IP-címet, a böngésző tootest webes kapcsolatot egy későbbi lépésben kapcsolatba léphet a tooit.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Használjon hello következő parancsot a toocreate egy távoli asztali munkamenet hello virtuális géppel. Cserélje le hello IP-cím hello *publicIPAddress* a virtuális gép. Amikor a rendszer kéri, adja meg a hello virtuális gép létrehozásakor használt hello hitelesítő adatokat.

```bash 
mstsc /v:<publicIpAddress>
```

## <a name="install-iis-via-powershell"></a>IIS telepítése a PowerShell használatával

Most, hogy az Azure virtuális gép toohello már bejelentkezett, használjon PowerShell tooinstall IIS egysoros, és hello helyi tűzfal szabály tooallow webes forgalom engedélyezése. Nyisson meg egy PowerShell-parancssorba, és futtassa a következő parancs hello:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a>Nézet hello IIS-kezdőlap

A telepített IIS-t, és most nyissa meg a virtuális gép hello Internet a 80-as porton az a choice tooview hello alapértelmezett IIS üdvözlőlap webböngésző is használhatja. Lehet, hogy toouse hello *publicIpAddress* toovisit hello alapértelmezett oldal fent részletes ismertetését lásd. 

![Alapértelmezett IIS-webhely](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szükség, használhatja a hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót. További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Windows virtuális gépek továbbra is.

> [!div class="nextstepaction"]
> [Windowsos virtuális gépek az Azure-ban – oktatóanyagok](./tutorial-manage-vm.md)
