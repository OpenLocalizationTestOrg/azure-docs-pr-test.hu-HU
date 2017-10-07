---
title: "aaaCreate és kezelése Windows virtuális gépek hello Azure PowerShell modul |} Microsoft Docs"
description: "Az oktatóanyag - létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 20adcb673ef4de683e6ad82d048a9625a1dc838c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-with-hello-azure-powershell-module"></a>Létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul

Az Azure virtuális gépek adjon meg egy teljes mértékben konfigurálhatók és rugalmas számítási környezet. Ez az oktatóanyag ismerteti alapszintű Azure-beli virtuális gép telepítési elemek, például Virtuálisgép-méret kiválasztása, Virtuálisgép-lemezkép kiválasztása és a virtuális gépek telepítése. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Hozzon létre, és csatlakozzon a virtuális gép tooa
> * Válassza ki és használja a Virtuálisgép-rendszerképek
> * Megtekintése és használata a megadott Virtuálisgép-méretek
> * Virtuális gép átméretezése
> * Megtekinteni és megérteni a virtuális gép állapota

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

## <a name="create-resource-group"></a>Erőforráscsoport létrehozása

Hozzon létre egy erőforráscsoportot hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsot. 

Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. Egy erőforráscsoportot a virtuális gépek előtt létre kell hozni. Ebben a példában az erőforráscsoport neve *myResourceGroupVM* jön létre az hello *EastUS* régióban. 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

hello erőforráscsoport létrehozása vagy módosítása egy virtuális Gépet, amelynek ez az oktatóanyag teljes látható során van megadva.

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

A virtuális gép csatlakoztatott tooa virtuális hálózaton kell lennie. Hello virtuális gép egy nyilvános IP-címet a hálózati kártya keresztül kommunikálnak.

### <a name="create-virtual-network"></a>Virtuális hálózat létrehozása

Hozzon létre egy alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

A virtuális hálózat létrehozása [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 ` 
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Nyilvános IP-cím létrehozása

Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```powershell
$pip = New-AzureRmPublicIpAddress ` 
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS ` 
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a>Hálózati kártya létrehozása

Hozzon létre egy hálózati kártya [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```powershell
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Hálózati biztonsági csoport létrehozása

Egy Azure [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) (NSG) szabályozza a bejövő és kimenő forgalmat egy vagy több virtuális géphez. Hálózati biztonsági csoportszabályok engedélyezheti vagy letilthatja a hálózati forgalmat egy adott portot vagy porttartományt. Ezek a szabályok is hozzáadhat egy forráscímelőtag, hogy csak egy előre meghatározott forrás származó forgalmat a virtuális gépek kommunikálhatnak. tooaccess hello IIS webkiszolgálót, amely telepíti, hozzá kell adnia egy NSG bejövő szabályt.

toocreate NSG bejövő szabály, használjon [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). hello következő például egy szabályt hoz létre NSG nevű *myNSGRule* portot nyit meg, amely *3389-es* hello virtuális géphez:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

Hozzon létre hello NSG-t használó *myNSGRule* rendelkező [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

Hello NSG toohello alhálózat hozzáadása a virtuális hálózat hello [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Frissítés hello virtuális hálózat [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Virtuális gép létrehozása

A virtuális gép létrehozásakor több lehetőség is elérhető, például az operációs rendszer lemezképét, a lemez méretezés és a felügyeleti hitelesítő adatokat. Ebben a példában egy virtuális gép létrehozása nevű *myVM* futó hello legújabb verzióját a Windows Server 2016 Datacenter.

Állítsa be a hello felhasználónév és jelszó szükséges hello rendszergazdai fiók hello virtuális gépen a [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Hozzon létre hello hello virtuális gép kezdeti konfigurálásához [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig):

```powershell
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

Adja hozzá a hello operációs rendszer információt toohello virtuálisgép-konfigurációnak [Set-AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):

```powershell
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Adja hozzá a hello kép információk toohello virtuálisgép-konfiguráció [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage):

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Adja hozzá a hello operációs rendszer lemez beállítások toohello virtuálisgép-konfigurációnak [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk):

```powershell
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Hozzáadás hello hálózati kártya korábban létrehozott toohello virtuálisgép-konfigurációnak [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Hozzon létre virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-toovm"></a>Csatlakozás tooVM

Hello telepítés befejeződése után létre kell hoznia egy távoli asztali kapcsolat hello virtuális géppel.

Futtassa a következő parancsok tooreturn hello nyilvános IP-cím hello virtuális gép hello. Jegyezze fel ezt az IP-címet, a böngésző tootest webes kapcsolatot egy későbbi lépésben kapcsolatba léphet a tooit.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

Használjon hello következő parancsot a toocreate egy távoli asztali munkamenet hello virtuális géppel. Cserélje le hello IP-cím hello *publicIPAddress* a virtuális gép. Amikor a rendszer kéri, adja meg a hello virtuális gép létrehozásakor használt hello hitelesítő adatokat.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>Virtuálisgép-rendszerképek ismertetése

hello Azure piactér lehet használt toocreate egy új virtuális gép több virtuálisgép-lemezképeket tartalmaz. Hello előző lépésekben a virtuális gép létrehozása hello Windows Server 2016-Datacenter rendszerképet használ. Ebben a lépésben hello PowerShell modul az használt toosearch hello piactér más Windows-lemezképek, amely is alapjaként új virtuális gépek esetén. Ezt a folyamatot, hogy a rendszer hello közzétevő, az ajánlat és hello kép neve (Sku) áll. 

Használjon hello [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) parancs tooreturn kép közzétevők listájának.  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Használjon hello [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) tooreturn kép ajánlatok listáját. Ezzel a paranccsal hello visszaadott lista a megadott közzétevő hello szűrve van. 

```powershell
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```powershell
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

Hello [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) parancs majd szűrést hello közzétevő és ajánlat tooreturn kép neveinek listáját.

```powershell
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```powershell
Skus                            Offer         PublisherName          Location
----                            -----         -------------          --------
2008-R2-SP1                     WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-BYOL                WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-BYOL            WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter              WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-BYOL         WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                 WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core     WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-with-Containers WindowsServer MicrosoftWindowsServer EastUS  
2016-Nano-Server                WindowsServer MicrosoftWindowsServer EastUS
```

Ezek az információk használt toodeploy azokba rendelkező virtuális gépeknél lehet. Ez a példa hello Virtuálisgép-objektum hello lemezképnév beállítása. Tekintse meg az előző példák toohello ebben az oktatóanyagban a teljes telepítési lépéseket.

```powershell
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>Virtuálisgép-méretek ismertetése

A virtuális gép méretét hello számítási erőforrásokat, elérhető toohello virtuális gépen végrehajtott például a CPU, grafikus Processzor és memória mennyisége határozza meg. Virtuális gépek kell toobe létrehozott méretű megfelelő hello várható terhelése. Ha növeli a munkaterhelés, egy meglévő virtuális gépet is átméretezhetők.

### <a name="vm-sizes"></a>Virtuálisgép-méretek

a következő táblázat hello méretek kategorizálja a használati esetek.  

| Típus                     | Méretek           |    Leírás       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Általános célú         |DSv2, Dv2, DS, D, Av2, A0-7| Elosztott terhelésű CPU-memória. Épp ezért tökéletes választás a fejlesztési / tesztelési és kis toomedium alkalmazások és adatok megoldások.  |
| Számításra optimalizált      | FS, F             | Magas CPU-memória. Jó közepes forgalom alkalmazásokat, hálózati berendezéseket és kötegelt folyamatok.        |
| Memóriaoptimalizált       | GS, G, DSv2, DS, Dv2, D   | Magas memória-core. A relációs adatbázisok, közepes méretű toolarge gyorsítótárak és memórián belüli analytics nagy.                 |
| Tárolásra optimalizált       | Ls                | Magas lemez-adatátviteli és I/O-műveleti jellemzők. Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.                                                         |
| GPU           | PORTOK HV, NC            | Nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt speciális virtuális gépeket.       |
| Kiemelkedő teljesítmény | H, A8-11          | A leghatékonyabb CPU rendelkező virtuális gépek nagy átviteli további hálózati adapterek (RDMA). 


### <a name="find-available-vm-sizes"></a>Elérhető Virtuálisgép-méretek keresése

virtuális gép listájának toosee méretének egy adott régióban, használja a hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) parancsot.

```powershell
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a>Virtuális gép átméretezése

A virtuális gépek telepítése után az átméretezett tooincrease kell, vagy erőforrás-elosztás csökkentése.

A virtuális gépek átméretezésével előtt ellenőrizze, hogy hello kívánt mérete hello aktuális Virtuálisgép-fürt érhető el. Hello [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) parancs méretek listáját adja vissza. 

```powershell
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

Hello szükséges méret érhető el, ha hello VM átméretezhető az alkalmazás bekapcsolja a állapotból, azonban hello művelet során újraindítása után.

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

Hello méretének igény nincs fürtön hello aktuális hello virtuális gép igényeinek felszabadítása előtt hello átméretezése művelet toobe akkor fordulhat elő. Vegye figyelembe, hello VM regisztráló vissza, bármely hello ideiglenes lemezen lévő adatok törlődnek, és a hello nyilvános IP-cím módosítása, kivéve, ha a statikus IP-cím használatban van. 

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a>Virtuális gép energiaállapotokat

Egy Azure virtuális gép sok energiaállapotát egyike lehet. Ebben az állapotban hello hello virtuális gép aktuális állapotát jeleníti meg hello hipervizor hello tükrözik. 

### <a name="power-states"></a>Energiaállapotokat

| Energiaállapot | Leírás
|----|----|
| Indulás alatt | Azt jelzi, hogy hello virtuális gép elindul. |
| Fut | Azt jelzi, hogy hello a virtuális gép futása. |
| Leállítás | Azt jelzi, hogy hello a virtuális gép leáll. | 
| Leállítva | Azt jelzi, hogy hello a virtuális gép le van állítva. Vegye figyelembe, hogy a virtuális gépek hello leállt állapotban továbbra is fel Önnek költségeket.  |
| Felszabadítása | Azt jelzi, hogy hello a virtuális gép felszabadítása. |
| Felszabadítása | Azt jelzi, hogy hello a virtuális gép teljesen eltávolítja a hello hipervizor, de továbbra is elérhetők a hello vezérlő vezérlősík. Hello Deallocated állapotban lévő virtuális gépek nem számítunk költségeket. |
| - | Azt jelzi, hogy hello power hello virtuális gép állapota ismeretlen. |

### <a name="find-power-state"></a>Energiaállapot keresése

egy adott virtuális Gépet, használjon hello tooretrieve hello állapotának [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) parancsot. Lehet, hogy toospecify egy érvényes nevet a virtuális gép és erőforráscsoport. 

```powershell
Get-AzureRmVM `
    -ResourceGroupName myResourceGroup `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Kimenet:

```powershell
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Felügyeleti feladatok

A virtuális gépek hello életciklusa során szükség lehet a toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése. Emellett érdemes lehet toocreate parancsfájlok tooautomate ismétlődő vagy összetett feladatokat. Az Azure PowerShell használatával számos gyakori felügyeleti feladatokat is futtatható hello parancssorból vagy parancsfájlokban.

### <a name="stop-virtual-machine"></a>Virtuális gép leállítása

Állítsa le és a virtuális gép felszabadítása [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

Ha azt szeretné, hogy tookeep hello virtuális gép kiosztott állapotú, használja a hello - StayProvisioned paramétert.

### <a name="start-virtual-machine"></a>Virtuális gép elindítása

```powershell
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a>Erőforráscsoport törlése

Erőforráscsoport törlésekor a belül található összes erőforrást is törlődnek.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban megismerte alapvető virtuális gép létrehozása és kezelése például hogyan:

> [!div class="checklist"]
> * Hozzon létre, és csatlakozzon a virtuális gép tooa
> * Válassza ki és használja a Virtuálisgép-rendszerképek
> * Megtekintése és használata a megadott Virtuálisgép-méretek
> * Virtuális gép átméretezése
> * Megtekinteni és megérteni a virtuális gép állapota

Virtuális gép lemezeivel kapcsolatos útmutató toolearn következő toohello előzetes.  

> [!div class="nextstepaction"]
> [Hozzon létre és kezelheti a virtuális gépek lemezei](./tutorial-manage-data-disk.md)
