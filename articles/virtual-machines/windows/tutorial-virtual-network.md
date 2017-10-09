---
title: "aaaAzure virtuális hálózatok és a Windows virtuális gépek |} Microsoft Docs"
description: "Az oktatóanyag - kezelése az Azure virtuális hálózatok és a Windows virtuális gépek az Azure PowerShell használatával"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: ed77d9d5873e849fcb2aaf15e41899d7ad8c781a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Egy Azure virtuális hálózatot és az Azure PowerShell használatával Windows virtuális gépek kezelése

Az Azure virtuális gépek Azure hálózatkezelés belső és külső hálózati kommunikációhoz használni. Ebben az oktatóanyagban hozzon létre több virtuális gép (VM) virtuális hálózatban, és konfigurálja őket közötti hálózati kapcsolat. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Virtuális hálózat létrehozása
> * Virtuális hálózati alhálózat létrehozása
> * Szabályozza a hálózati forgalmat hálózati biztonsági csoportokkal
> * A művelet forgalmi szabályok megtekintése

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

## <a name="create-vnet"></a>VNet létrehozása

A virtuális hálózat a saját hálózati hello felhőben megjelenítése. A virtuális hálózat hello Azure felhőben dedikált tooyour előfizetés logikai elkülönítése. Egy Vneten belül található alhálózatok felderítéséhez, a kapcsolat toothose alhálózatok és kapcsolatok hello virtuális gépek toohello alhálózatok szabályait.

Bármely más Azure-erőforrások létrehozása előtt kell toocreate nevű erőforráscsoport [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello alábbi példa létrehoz egy erőforráscsoportot *myRGNetwork* a hello *EastUS* helye:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

Egy alhálózat egy Vnetet gyermek erőforrása, és segítségével meghatározhatja a címterek belül használja az IP-cím előtagokat CIDR-blokkja részeit. Hálózati adapter toosubnets, és a csatlakoztatott tooVMs biztosítható a kapcsolat a különböző munkaterhelések lehet hozzáadni.

Hozzon létre egy alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```powershell
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

Hozzon létre egy VNETET nevű *myVNet* használatával *myFrontendSubnet* rendelkező [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet
```

## <a name="create-front-end-vm"></a>Előtér-virtuális gép létrehozása

A virtuális gép toocommunicate a Vneten belül, a virtuális hálózati kártya (NIC) szükséges. Hello *myFrontendVM* hello elérése internet, így kell továbbá egy nyilvános IP-címet. 

Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

Hozzon létre egy hálózati Adaptert a [új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):


```powershell
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Hello felhasználónév és jelszó szükséges a virtuális gép hello hello rendszergazdai fiók beállítása az [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Hozzon létre virtuális gépek hello [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Hozzáadása AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), és [új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). 

```powershell
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="install-web-server"></a>Webalkalmazás-kiszolgáló telepítése

Telepítheti az IIS *myFrontendVM* távoli asztali kapcsolat segítségével. Tooget hello nyilvános IP-címe hello VM tooaccess kell azt.

Hello nyilvános IP-címe kaphat *myFrontendVM* rendelkező [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello alábbi példa beszerzi hello IP-címet *myPublicIPAddress* korábban létrehozott:

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myRGNetwork `
    -Name myPublicIPAddress | select IpAddress
```

Jegyezze fel ezt az IP-címet, hogy használhassa a későbbi lépésekben.

Használjon hello következő parancsot a távoli asztali munkamenetet a toocreate *myFrontendVM*. Cserélje le  *<publicIPAddress>*  korábban feljegyzett hello címmel. Amikor a rendszer kéri, adja meg a hello virtuális gép létrehozásakor használt hello hitelesítő adatokat.

```
mstsc /v:<publicIpAddress>
``` 

Most, hogy már bejelentkezett túl*myFrontendVM*, használhatja a PowerShell tooinstall IIS egysoros és hello helyi tűzfal szabály tooallow webes forgalom engedélyezése. Nyisson meg egy PowerShell-parancssorba, és futtassa a következő parancs hello:

Használjon [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/servermanager/install-windowsfeature) toorun hello egyéni parancsprogramok futtatására szolgáló bővítmény, amely telepíti az IIS-webkiszolgáló hello:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Most hello nyilvános IP cím toobrowse toohello VM toosee hello IIS-webhely is használhatja.

![Alapértelmezett IIS-webhely](./media/tutorial-virtual-network/iis.png)

## <a name="manage-internal-traffic"></a>Belső forgalom kezelése

A hálózati biztonsági csoport (NSG) szabályokat, amelyek engedélyezik vagy megtagadják a hálózati forgalom csatlakoztatott tooresources tooa hálózatok listáját tartalmazza. NSG-ket lehet társított toosubnets, vagy az egyes hálózati adapterek csatlakoztatva tooVMs. Megnyitása vagy bezárása hozzáférés tooVMs portokon keresztül történik NSG-szabályok használatával. Létrehozási *myFrontendVM*, 3389-es port bejövő automatikusan nyitották meg az RDP-kapcsolatot.

A virtuális gépek belső kommunikációs egy NSG használatával konfigurálható. Ebben a szakaszban megismerheti, hogyan toocreate egy további alhálózati hello a hálózatot, és rendelje hozzá egy NSG tooit tooallow kapcsolatot *myFrontendVM* túl*myBackendVM* 1433-as portot. hello alhálózati majd toohello VM van hozzárendelve, létrehozásakor.

Belső forgalom korlátozhatja túl*myBackendVM* csak *myFrontendVM* hozzon létre egy NSG-t hello háttér-alhálózatot. hello következő például egy szabályt hoz létre NSG nevű *myBackendNSGRule* rendelkező [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

```powershell
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Adja hozzá a hálózati biztonsági csoport nevű *myBackendNSG* rendelkező [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```powershell
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```
## <a name="add-back-end-subnet"></a>Háttér-alhálózat hozzáadása

Hozzáadás *myBackEndSubnet* túl*myVNet* rendelkező [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):

```powershell
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -VirtualNetwork $vnet `
  -AddressPrefix 10.0.1.0/24 `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
```

## <a name="create-back-end-vm"></a>Háttér-virtuális gép létrehozása

hello legegyszerűbb módja toocreate hello háttér-virtuális gép van egy SQL Server-lemezkép használatával. Ez az oktatóanyag csak hoz létre a virtuális gép hello hello adatbázis-kiszolgáló, de nem adja meg a hello adatbázis elérésével kapcsolatos információkat.

Hozzon létre *myBackendNic*:

```powershell
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Állítsa be a hello felhasználónév és a virtuális gép és a Get-Credential hello hello rendszergazdai fiók szükséges jelszót:

```powershell
$cred = Get-Credential
```

Hozzon létre *myBackendVM*:

```powershell
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

hello kép használt SQL Server telepítve van, de nem szerepel ebben az oktatóanyagban. Fontos a része tooshow, hogyan konfigurálhat egy virtuális gép toohandle webes forgalom és a virtuális gép toohandle adatbázisának kezelése.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban létre, és biztonságos kapcsolódó toovirtual gépként Azure hálózatokhoz. 

> [!div class="checklist"]
> * Virtuális hálózat létrehozása
> * Virtuális hálózati alhálózat létrehozása
> * Szabályozza a hálózati forgalmat hálózati biztonsági csoportokkal
> * A művelet forgalmi szabályok megtekintése

A virtuális gépek az Azure backup használatával biztonságossá tétele adatok figyelésével kapcsolatos útmutató toolearn következő toohello előzetes. .

> [!div class="nextstepaction"]
> [Készítsen biztonsági másolatot a Windows virtuális gépek Azure-ban](./tutorial-backup-vms.md)
