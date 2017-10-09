---
title: "aaaTutorial - Build egy magas rendelkezésre állású alkalmazások az Azure virtuális gépeken futó |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy magas rendelkezésre állású és biztonságos alkalmazás három Windows virtuális gépek, az Azure-ban terheléselosztót között"
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
ms.date: 03/30/2017
ms.author: davidmu
ms.openlocfilehash: f9eff96be4f3999651c4108f0334e4eaa1a39c0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a>Egy elosztott terhelésű, a Windows Azure virtuális gépek magas rendelkezésre állású alkalmazás létrehozása

Ebben az oktatóanyagban létrehozhat egy magas rendelkezésre állású alkalmazás, amely rugalmas toomaintenance események. hello alkalmazás használ, a terheléselosztó egy rendelkezésre állási csoportot és három Windows virtuális gépek (VM). Ez az oktatóanyag az IIS-t telepíti, használhatja, ha az oktatóanyag egy másik alkalmazási keretrendszer használatával toodeploy hello azonos magas rendelkezésre állású összetevők és útmutatást. 

## <a name="step-1---azure-prerequisites"></a>1. lépés – Azure-Előfeltételek

toocomplete ebben az oktatóanyagban győződjön meg arról, hogy telepítette hello legújabb [Azure PowerShell](/powershell/azure/overview) modul.

Első lépésként jelentkezzen be Azure előfizetés hello Login-AzureRmAccount parancs tooyour, és kövesse hello képernyőn megjelenő utasításokat.

```powershell
Login-AzureRmAccount
```

Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. Bármely más Azure-erőforrások létrehozása előtt kell toocreate nevű erőforráscsoport [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` régió: 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a>2. lépés - a rendelkezésre állási csoport létrehozása

Virtuális gépek közötti logikai tartalék hozhatók létre, és a tartományok frissítése. Minden logikai tartomány jelöli az alapul szolgáló Azure datacenter hello hardver egy része. Két vagy több virtuális gép létrehozásakor a számítási és tárolási erőforrásokat ezekből a tartományokból különböző pontjain. Ehhez a terjesztéshez hello rendelkezésre állását az alkalmazás kezeli, ezért ha egy hardverösszetevő karbantartási. Rendelkezésre állási készletek lehetővé teszik, hogy a logikai hiba és a frissítési tartományok definiálása.

Állítsa be a rendelkezésre állási létrehozása [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett `myAvailabilitySet`:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a>3. lépés - betöltési létrehozása terheléselosztó

Egy Azure terheléselosztó a megadott virtuális gépeket használ a load balancer szabályok készletét forgalom elosztása. Egy állapotmintáihoz az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat tooan működési virtuális gép.

### <a name="create-public-ip-address"></a>Nyilvános IP-cím létrehozása

tooaccess hello Internet, az alkalmazás hozzárendelése egy nyilvános IP cím toohello terheléselosztó. Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). hello alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP`:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a>Terheléselosztó létrehozása

Hozzon létre egy előtérbeli IP-cím [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). hello alábbi példa létrehoz egy előtérbeli IP-cím nevű `myFrontEndPool`: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

Hozzon létre egy háttér-címkészlet, amely [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). hello alábbi példa létrehoz egy háttér címkészletet nevű `myBackEndPool`:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Ezután hozzon létre hello rendelkező terheléselosztó [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer` hello segítségével `myPublicIP` cím:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a>Állapotfigyelő mintavétel létrehozása

tooallow hello terheléselosztó toomonitor hello állapotához az alkalmazás, használhat egy állapotmintáihoz. hello állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek hello terhelés terheléselosztó Elforgatás a válasz toohealth ellenőrzések alapján. Alapértelmezés szerint a virtuális gépek terheléselosztó terheléselosztási hello 15 másodperces időközönként két egymást követő hibák után törlődik.

Hozzon létre egy állapotmintáihoz rendelkező [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). hello alábbi példa létrehoz egy állapotmintáihoz nevű `myHealthProbe` , amely figyeli az egyes virtuális gépek:

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a>Terheléselosztási szabály létrehozása

Olyan terheléselosztó szabályhoz használt toodefine forgalom Mitől elosztott toohello virtuális gépeket.

Hozzon létre olyan terheléselosztó szabályhoz rendelkező [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). hello alábbi példa létrehoz egy terheléselosztási szabály nevű `myLoadBalancerRule` és porton forgalom egyenlege `80`:

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

Hello rendelkező terheléselosztó frissítése [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a>4. lépés - a hálózatkezelés konfigurálását

Minden virtuális gép rendelkezik legalább egy virtuális hálózati adapterek (NIC) tooa virtuális hálózat. Ez a virtuális hálózat védett toofilter forgalom meghatározott hozzáférési szabályok alapján.

### <a name="create-virtual-network"></a>Virtuális hálózat létrehozása

Először is konfiguráljon egy alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). hello alábbi példa létrehoz egy nevű alhálózat `mySubnet`:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

tooprovide hálózati kapcsolat tooyour virtuális gépeket, a virtuális hálózat létrehozása [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). hello alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` rendelkező `mySubnet`:

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a>Konfigurálja a hálózati biztonság

Egy Azure [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) (NSG) szabályozza a bejövő és kimenő forgalmat egy vagy több virtuális géphez. Hálózati biztonsági csoportszabályok engedélyezheti vagy letilthatja a hálózati forgalmat egy adott portot vagy porttartományt. Ezek a szabályok is hozzáadhat egy forráscímelőtag, hogy csak egy előre meghatározott forrás származó forgalmat a virtuális gépek kommunikálhatnak.

tooallow webes forgalom tooreach az alkalmazás, hozzon létre egy hálózati biztonsági csoport szabály [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). hello alábbi példa létrehoz egy hálózati biztonsági szabály nevű `myNetworkSecurityGroupRule`:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

Hozzon létre egy hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). hello alábbi példa létrehoz egy NSG nevű `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

Hello hálózati biztonsági csoport toohello alhálózat hozzáadása [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

Frissítés hello virtuális hálózat [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a>Virtuális hálózati adapterek létrehozása

Terheléselosztók függvény hello virtuális hálózati adapter erőforrás betölteni, hanem tényleges VM hello. hello a virtuális hálózati adapter csatlakoztatott toohello terheléselosztóhoz, és majd csatolni a virtuális gép tooa.

Hozzon létre egy virtuális hálózati adapter a [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). hello alábbi példakód létrehozza három virtuális hálózati adapter. (Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre a lépések az alkalmazást a következő hello):


```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroup `
     -Name myNic$i `
     -Location westeurope `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}

```

## <a name="step-5---create-virtual-machines"></a>5. lépés - a virtuális gépek létrehozása

Az összes hello alapjául szolgáló összetevőket helyen most létrehozhat magas rendelkezésre állású virtuális gépek toorun az alkalmazást. 

Hello felhasználónév és jelszó szükséges hello virtuális gépen a hello rendszergazdai fiók beszerzése [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Hozzon létre virtuális gépek hello [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [Hozzáadása AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), és [új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). a következő példa hello három virtuális gépeket hoz létre:

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -StorageAccountType StandardLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vm
}

```

Néhány perc toocreate vesz igénybe, és minden három virtuális gép konfigurálása. hello terheléselosztó automatikusan észleli, ha egyes virtuális gépek hello az alkalmazás fut.. Miután hello alkalmazás fut, a hello terheléselosztási szabály elindít toodistribute forgalmat.

### <a name="install-hello-app"></a>Hello alkalmazás telepítéséhez 

Az Azure virtuálisgép-bővítmények használt tooautomate virtuális gép konfigurációs feladatokhoz, mint az alkalmazások telepítése és konfigurálása hello operációs rendszer. Hello [egyéni parancsprogramok futtatására szolgáló bővítmény Windows](./../virtual-machines-windows-extensions-customscript.md) használt toorun van a PowerShell-parancsfájl hello virtuális gépen. hello parancsfájl az Azure storage, bármely elérhető HTTP-végpont tárolja, vagy beágyazott hello egyéni parancsfájl bővítménykonfiguráció. Hello egyéni parancsprogramok futtatására szolgáló bővítmény használatakor hello Azure Virtuálisgép-ügynök kezelése hello parancsfájl végrehajtása.

Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello egyéni parancsprogramok futtatására szolgáló bővítmény. bővítmény futtatása hello `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webkiszolgálón:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
     -Location westeurope
}
```

### <a name="test-your-app"></a>Az alkalmazás tesztelése

Hello nyilvános IP-cím a terheléselosztó az beszerzése [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello alábbi példa beszerzi hello IP-címet `myPublicIP` korábban létrehozott:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

Adja meg a nyilvános IP-cím hello tooa webböngészőben. Hello NSG szabállyal helyen hello alapértelmezett IIS-webhely jelenik meg. 

![Alapértelmezett IIS-webhely](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a>6. lépés – felügyeleti feladatok

Szükség lehet az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket hello tooperform karbantartása. toodeal megnövekedett forgalom tooyour alkalmazással, szükség lehet tooadd további virtuális gépeket. Ez a szakasz bemutatja, hogyan tooremove, vagy adja hozzá a virtuális gépek hello terheléselosztóról. 

### <a name="remove-a-vm-from-hello-load-balancer"></a>Távolítsa el a virtuális gépek hello terheléselosztó

Távolítsa el a virtuális gépek hello háttércímkészletet hello hálózati kártya konfigurációja terheléselosztói Háttércímkészletet tulajdonságának hello alaphelyzetbe állításával.

Hálózati kártya hello az beszerzése [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

Hello hálózati kártya hello konfigurációja terheléselosztói Háttércímkészletet tulajdonságának beállítása túl$ null:

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

Frissítés hello hálózati kártya:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-toohello-load-balancer"></a>Virtuális gép toohello terheléselosztó felvétele

VM karbantartásának végrehajtása, vagy tooexpand kapacitás van szüksége, ha egy virtuális gép toohello háttér címkészletet hello terheléselosztó hálózati Adapterének hello hozzáadása.

Hello terheléselosztó beolvasása:

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

Hello háttér címkészletet hello load balancer toohello hálózati kártya hozzáadása:

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

Frissítés hello hálózati kártya:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Következő lépések

Minták – [Azure virtuális gép PowerShell-mintaparancsfájlok](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
